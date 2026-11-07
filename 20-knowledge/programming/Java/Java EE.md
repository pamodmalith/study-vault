## Java EE Versions Comparison

| **Feature / API**                                 | **Java EE 6** (2009) | **Java EE 7** (2013)                      | **Java EE 8** (2017)                             |
| ------------------------------------------------- | -------------------- | ----------------------------------------- | ------------------------------------------------ |
| **Servlet**                                       | 3.0                  | 3.1 (non-blocking I/O, protocol upgrades) | 4.0 (HTTP/2 support)                             |
| **JSP**                                           | 2.2                  | 2.3                                       | 2.3                                              |
| **EL (Expression Language)**                      | 2.2                  | 3.0                                       | 3.0                                              |
| **JPA (Persistence)**                             | 2.0                  | 2.1 (stored procedures, schema gen)       | 2.2 (stream API, Java 8 Date/Time)               |
| **JSF (Faces)**                                   | 2.0                  | 2.2 (HTML5, resource library contracts)   | 2.3 (CDI alignment, push communication)          |
| **EJB (Enterprise Java Beans)**                   | 3.1                  | 3.2 (simplified packaging)                | 3.2                                              |
| **JAX-RS (REST)**                                 | 1.1                  | 2.0 (client API, async processing)        | 2.1 (reactive client, server-sent events)        |
| **JAX-WS (SOAP)**                                 | 2.2                  | 2.2                                       | 2.2                                              |
| **CDI (Dependency Injection)**                    | 1.0                  | 1.1 (portable extensions, interceptors)   | 2.0 (Java 8 integration, async events)           |
| **Bean Validation**                               | 1.0                  | 1.1 (method-level validation)             | 2.0 (Java 8 Date/Time, repeatable annotations)   |
| **WebSocket API**                                 | ❌ (not available)    | ✅ 1.0 (new in EE7)                        | ✅ 1.1 (updates)                                  |
| **JSON Processing (JSON-P)**                      | ❌                    | ✅ 1.0 (parsing, generation)               | ✅ 1.1 (JSON Pointer, Patch, streaming)           |
| **JSON Binding (JSON-B)**                         | ❌                    | ❌                                         | ✅ 1.0 (object ↔ JSON mapping, like Jackson/Gson) |
| **Concurrency Utilities**                         | ❌                    | ✅ 1.0                                     | ✅ 1.0                                            |
| **Batch Processing**                              | ❌                    | ✅ 1.0                                     | ✅ 1.0                                            |
| **Default DataSource in `@DataSourceDefinition`** | ❌                    | ❌                                         | ✅ (EE 8 enhancement)                             |
