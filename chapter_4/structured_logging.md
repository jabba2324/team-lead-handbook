# Structured Logging

Structured logging is an approach to logging where log entries are formatted as structured data (typically JSON) rather than plain text strings. This makes logs machine-readable, searchable, and more valuable for monitoring, troubleshooting, and analysis.

## Core Principles

### 1. Machine-Readable Format
- Use structured formats like JSON or XML
- Consistent field names across all log entries
- Avoid embedding structured data within free-text fields

### 2. Consistent Schema
- Define a core schema for all log entries
- Allow for extensibility with context-specific fields
- Document the schema for developers and operations teams

### 3. Semantic Logging
- Log events, not just messages
- Include business context where appropriate
- Use consistent terminology across the system

## Essential Log Fields

Every log entry should include these minimum fields:

### 1. Identification
- **Trace ID**: Unique identifier that follows a request through the entire system
- **Span ID**: Identifier for a specific operation within a trace
- **Request ID**: Unique identifier for the incoming request
- **Session ID**: Identifier for the user's session (where applicable)

### 2. Temporal Information
- **Timestamp**: ISO 8601 format with timezone (e.g., `2023-06-15T08:42:15.505Z`)
- **Duration**: Time taken to complete the operation (in milliseconds)

### 3. Context
- **Service Name**: The service generating the log
- **Environment**: Production, staging, development, etc.
- **Host/Instance**: Server or container identifier
- **Version**: Application version or build number

### 4. User Context (Anonymized)
- **User ID**: Hashed or tokenized identifier (never PII)
- **Tenant ID**: For multi-tenant applications
- **Organization ID**: Business context where applicable
- **Role/Permissions**: User's access level (when relevant to the event)

### 5. Event Details
- **Log Level**: ERROR, WARN, INFO, DEBUG, TRACE
- **Event Type/Category**: Classification of the event (e.g., `authentication`, `database`, `api`)
- **Event Name**: Specific action or event (e.g., `user_login_attempt`, `payment_processed`)
- **Message**: Human-readable description
- **Code Location**: Class, function, file, and line number

### 6. Error Information
- **Error Type**: Exception class or error code
- **Error Message**: Description of what went wrong
- **Stack Trace**: Full stack trace (for errors)
- **Cause**: Root cause information

### 7. Technical Context
- **HTTP Method**: GET, POST, PUT, DELETE, etc.
- **URL Path**: Request path (sanitized of sensitive data)
- **Status Code**: HTTP response code
- **Component**: Specific component within the service

## Example Structured Log Entry

```json
{
  "timestamp": "2023-06-15T08:42:15.505Z",
  "level": "ERROR",
  "service": "payment-service",
  "environment": "production",
  "version": "1.2.3",
  "traceId": "abc123def456",
  "spanId": "span456",
  "requestId": "req789",
  "userId": "u_a7f8d9e3b2c1",
  "tenantId": "tenant_xyz",
  "eventType": "payment_processing",
  "eventName": "payment_authorization_failed",
  "message": "Payment authorization failed due to insufficient funds",
  "duration": 345,
  "httpMethod": "POST",
  "path": "/api/v1/payments",
  "statusCode": 400,
  "errorType": "PaymentDeclinedException",
  "errorMessage": "The payment was declined by the payment processor",
  "stackTrace": "com.example.PaymentService.authorize(PaymentService.java:125)...",
  "paymentProvider": "stripe",
  "paymentAmount": 99.99,
  "paymentCurrency": "USD"
}
```

## Implementation Best Practices

### 1. Use Logging Libraries with Structured Logging Support
- **Java**: Logback with Logstash encoder, Log4j2 with JSON layout
- **JavaScript/Node.js**: Winston, Bunyan, Pino
- **Python**: structlog, python-json-logger
- **Go**: Zap, Logrus
- **C#/.NET**: Serilog, NLog with structured logging extensions

### 2. Correlation IDs
- Generate trace IDs at the entry point of your system
- Propagate trace IDs through all services
- Include trace IDs in all logs related to a request
- Use standard headers like `X-Request-ID` or `X-Trace-ID`

### 3. Sensitive Data Handling
- Never log passwords, tokens, or credentials
- Hash or tokenize personally identifiable information (PII)
- Consider compliance requirements (GDPR, HIPAA, etc.)
- Implement field-level redaction for sensitive data

### 4. Performance Considerations
- Use asynchronous logging where possible
- Buffer logs to reduce I/O overhead
- Consider sampling for high-volume debug logs
- Optimize serialization performance

### 5. Log Levels
- **ERROR**: System errors requiring immediate attention
- **WARN**: Potential issues that don't stop the system
- **INFO**: Normal but significant events
- **DEBUG**: Detailed information for debugging
- **TRACE**: Very detailed debugging information

## Centralized Logging Architecture

### 1. Collection
- Use agents to collect logs from services (Fluentd, Filebeat, etc.)
- Stream logs directly from applications to log aggregators
- Implement buffering and retry mechanisms

### 2. Processing
- Parse and validate structured logs
- Enrich logs with additional metadata
- Filter out unnecessary information

### 3. Storage
- Use specialized log storage solutions (Elasticsearch, Loki, etc.)
- Implement retention policies based on log importance
- Consider hot/warm/cold storage tiers

### 4. Analysis and Visualization
- Use tools like Kibana, Grafana, or Splunk for visualization
- Create dashboards for common use cases
- Implement alerts based on log patterns

## Examples

### Java with Logback and Logstash Encoder

```java
import net.logstash.logback.argument.StructuredArguments;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.slf4j.MDC;

public class PaymentService {
    private static final Logger logger = LoggerFactory.getLogger(PaymentService.class);
    
    public void processPayment(Payment payment) {
        MDC.put("traceId", payment.getTraceId());
        MDC.put("userId", anonymizeUserId(payment.getUserId()));
        
        try {
            logger.info("Processing payment", 
                StructuredArguments.keyValue("paymentId", payment.getId()),
                StructuredArguments.keyValue("amount", payment.getAmount()),
                StructuredArguments.keyValue("currency", payment.getCurrency())
            );
            
            // Payment processing logic
            
        } catch (PaymentException e) {
            logger.error("Payment processing failed", 
                StructuredArguments.keyValue("errorCode", e.getCode()),
                StructuredArguments.keyValue("paymentId", payment.getId()),
                e
            );
            throw e;
        } finally {
            MDC.clear();
        }
    }
    
    private String anonymizeUserId(String userId) {
        // Implement anonymization logic
        return "u_" + hashFunction(userId);
    }
}
```

### Node.js with Winston

```javascript
const winston = require('winston');
const { v4: uuidv4 } = require('uuid');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  defaultMeta: { service: 'payment-service' },
  transports: [
    new winston.transports.Console()
  ]
});

function processPayment(payment, req) {
  const traceId = req.headers['x-trace-id'] || uuidv4();
  const logContext = {
    traceId,
    userId: anonymizeUserId(payment.userId),
    paymentId: payment.id,
    amount: payment.amount,
    currency: payment.currency
  };
  
  logger.info('Processing payment', logContext);
  
  try {
    // Payment processing logic
    logger.info('Payment processed successfully', {
      ...logContext,
      duration: calculateDuration()
    });
  } catch (error) {
    logger.error('Payment processing failed', {
      ...logContext,
      errorType: error.name,
      errorMessage: error.message,
      stackTrace: error.stack
    });
    throw error;
  }
}
```

## Sources

- [Elastic Common Schema (ECS)](https://www.elastic.co/guide/en/ecs/current/index.html)
- [OpenTelemetry Specification](https://opentelemetry.io/docs/reference/specification/)
- [Google's Dapper Paper](https://research.google/pubs/pub36356/)
- [The Twelve-Factor App: Logs](https://12factor.net/logs)
- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [AWS Well-Architected Framework: Observability](https://docs.aws.amazon.com/wellarchitected/latest/operational-excellence-pillar/observability.html)
- [Cindy Sridharan's "Distributed Systems Observability"](https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/)

[Back to Table of Contents](/README.md)