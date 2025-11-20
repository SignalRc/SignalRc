# Protocol Documentation (v1.0.0)

## Overview
The **Protocol** is a lightweight JSON‑based model and communication format inspired by Signal K, designed specifically for **radio‑controlled vehicles**.  
It supports:

- Real‑time telemetry  
- Position data  
- UUID‑based vehicle identities  
- A full state model  
- A delta update model  

All messages are transmitted as UTF‑8 JSON.

---

## Identity Model (UUID)
Each vehicle is uniquely identified using a **UUID v4**, for example:

```
705f5f1a-efaf-44aa-9cb8-a0fd6305567c
```

The field `self` indicates the UUID of the local vehicle.

---

# 1. Full State Model (Example)

```json
{
  "version": "1.0.0",
  "self": "705f5f1a-efaf-44aa-9cb8-a0fd6305567c",
  "cars": {
    "705f5f1a-efaf-44aa-9cb8-a0fd6305567c": {
      "name": "Buggy",
      "telemetry": {
        "speed":      { "value": 3.2,  "unit": "m/s" },
        "rpm":        { "value": 3200 },
        "battery":    { "value": 7.2,  "unit": "V" },
        "signal":     { "value": 85,   "unit": "%" },
        "temperature":{ "value": 42.1, "unit": "C" }
      },
      "position": {
        "value": { "x": 12.4, "y": 8.9 }
      },
      "timestamp": "2025-01-01T12:00:00Z"
    }
  }
}
```

---

# 2. Delta Update Model (Example)

```json
{
  "context": "cars.705f5f1a-efaf-44aa-9cb8-a0fd6305567c",
  "updates": [
    {
      "timestamp": "2025-01-01T12:00:01Z",
      "values": [
        { "path": "telemetry.speed", "value": 4.0 },
        { "path": "telemetry.rpm",   "value": 3500 },
        { "path": "position",        "value": { "x": 13.1, "y": 9.4 } }
      ]
    }
  ]
}
```

---

# 3. JSON Schemas

The schemas below define the structure of the Full State Model and Delta Update Model.

## 3.1 Full State Model Schema (full_schema.json)

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "RC Telemetry Full State Model",
  "type": "object",
  "properties": {
    "version": {
      "type": "string"
    },
    "self": {
      "type": "string",
      "pattern": "^[0-9a-fA-F-]{36}$"
    },
    "cars": {
      "type": "object",
      "additionalProperties": {
        "type": "object",
        "properties": {
          "name": {
            "type": "string"
          },
          "telemetry": {
            "type": "object",
            "properties": {
              "speed": {
                "type": "object",
                "properties": {
                  "value": {
                    "type": "number"
                  },
                  "unit": {
                    "type": "string"
                  }
                },
                "required": [
                  "value"
                ]
              },
              "rpm": {
                "type": "object",
                "properties": {
                  "value": {
                    "type": "number"
                  }
                },
                "required": [
                  "value"
                ]
              },
              "battery": {
                "type": "object",
                "properties": {
                  "value": {
                    "type": "number"
                  },
                  "unit": {
                    "type": "string"
                  }
                },
                "required": [
                  "value"
                ]
              },
              "signal": {
                "type": "object",
                "properties": {
                  "value": {
                    "type": "number"
                  },
                  "unit": {
                    "type": "string"
                  }
                },
                "required": [
                  "value"
                ]
              },
              "temperature": {
                "type": "object",
                "properties": {
                  "value": {
                    "type": "number"
                  },
                  "unit": {
                    "type": "string"
                  }
                },
                "required": [
                  "value"
                ]
              }
            }
          },
          "position": {
            "type": "object",
            "properties": {
              "value": {
                "type": "object",
                "properties": {
                  "x": {
                    "type": "number"
                  },
                  "y": {
                    "type": "number"
                  },
                  "z": {
                    "type": "number"
                  }
                },
                "required": [
                  "x",
                  "y"
                ]
              }
            }
          },
          "timestamp": {
            "type": "string",
            "format": "date-time"
          }
        },
        "required": [
          "telemetry",
          "position",
          "timestamp"
        ]
      }
    }
  },
  "required": [
    "version",
    "self",
    "cars"
  ]
}
```

---

## 3.2 Delta Update Model Schema (delta_schema.json)

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "RC Telemetry Delta Update Model",
  "type": "object",
  "properties": {
    "context": {
      "type": "string"
    },
    "updates": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "timestamp": {
            "type": "string",
            "format": "date-time"
          },
          "values": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "path": {
                  "type": "string"
                },
                "value": {}
              },
              "required": [
                "path",
                "value"
              ]
            }
          }
        },
        "required": [
          "timestamp",
          "values"
        ]
      }
    }
  },
  "required": [
    "context",
    "updates"
  ]
}
```

---

## End
This protocol is free to use, extend, and integrate into RC projects of any scale.
