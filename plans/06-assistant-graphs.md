# Implementation Plan: Assistant Graphs Documentation

## Overview

Create comprehensive documentation for the Multi-Assistant Orchestration (Assistant Graphs) feature, which allows routing conversations between multiple specialized assistants.

## Files to Create

### 1. `assistant-graphs.mdx`

**Purpose:** Main conceptual guide for Assistant Graphs

**Sections:**

1. **Introduction**
   - What are Assistant Graphs?
   - Use cases:
     - Route sales inquiries to sales bot, support to support bot
     - Escalation paths to human agents
     - Language-based routing
     - Department-specific handling
   - Benefits over single-assistant approach

2. **Core Concepts**

   **Graphs:**
   - Container for nodes and edges
   - Organization-scoped
   - Can be assigned to phone numbers
   
   **Nodes:**
   - Represent individual assistants
   - Node types:
     - Entry Node: Starting point (required)
     - Standard Node: Regular assistant
     - Exit Node: Conversation end points
     - Escalation Node: Human transfer
   
   **Edges:**
   - Define transitions between nodes
   - Conditions determine when transitions occur
   - Priority system for conflict resolution

3. **Transition Conditions**

   | Type | Description | Example |
   |------|-------------|---------|
   | Intent-Based | Trigger on detected intents | "user wants to speak to sales" |
   | Natural Language | LLM evaluates condition | "user is frustrated or angry" |
   | Mathematical | Expression-based | "call_duration > 300" |
   | State Variables | Variable checks | "department == 'billing'" |

4. **Building a Graph**
   - Step-by-step guide using the UI
   - Visual Graph Builder overview
   - Adding nodes
   - Connecting nodes with edges
   - Setting conditions
   - Testing the graph

5. **Handoff Behavior**
   - Handoff sentences (what to say during transition)
   - Context sharing between assistants
   - Conversation history preservation
   - Seamless user experience

6. **Phone Number Assignment**
   - Assigning phone numbers to graphs (vs individual assistants)
   - Inbound call routing
   - Outbound call behavior

7. **Best Practices**
   - Start simple (2-3 assistants)
   - Clear transition conditions
   - Test all paths
   - Monitor transition analytics

8. **Example Graphs**

   **Customer Service Router:**
   ```
   Entry (Receptionist) 
     → Sales Assistant (intent: "sales")
     → Support Assistant (intent: "support")  
     → Billing Assistant (intent: "billing")
     → Human Agent (escalation)
   ```

   **Language Router:**
   ```
   Entry (Language Detector)
     → English Assistant (language: "en")
     → Spanish Assistant (language: "es")
     → French Assistant (language: "fr")
   ```

**Source Reference:** `Features.MD` Section 8, `app/services/assistant_graph_service.py`

---

### 2. `api-reference/assistant-graphs/create.mdx`

**Endpoint:** `POST /api/v1/assistant-graphs`

**Content:**
- Description
- Request body schema:
  ```json
  {
    "name": "Customer Service Router",
    "description": "Routes customers to appropriate department",
    "is_active": true,
    "nodes": [
      {
        "assistant_id": 1,
        "node_type": "entry",
        "position_x": 0,
        "position_y": 0,
        "config": {}
      },
      {
        "assistant_id": 2,
        "node_type": "standard",
        "position_x": 200,
        "position_y": 0,
        "config": {}
      }
    ],
    "edges": [
      {
        "source_node_index": 0,
        "target_node_index": 1,
        "condition_type": "intent",
        "condition_config": {
          "intents": ["sales", "purchase", "buy"]
        },
        "priority": 1,
        "handoff_sentence": "Let me connect you with our sales team."
      }
    ]
  }
  ```
- Response schema
- Example request/response

**Source Reference:** `app/api/assistant_graphs.py`, `app/api/schemas.py`

---

### 3. `api-reference/assistant-graphs/list.mdx`

**Endpoint:** `GET /api/v1/assistant-graphs`

**Content:**
- Description
- Query parameters (skip, limit, active_only)
- Response schema (list of graphs)
- Example

---

### 4. `api-reference/assistant-graphs/get.mdx`

**Endpoint:** `GET /api/v1/assistant-graphs/{graph_id}`

**Content:**
- Description
- Path parameters
- Response schema (full graph with nodes and edges)
- Example

---

### 5. `api-reference/assistant-graphs/update.mdx`

**Endpoint:** `PUT /api/v1/assistant-graphs/{graph_id}`

**Content:**
- Description
- Path parameters
- Request body (partial update)
- Response schema
- Example

---

### 6. `api-reference/assistant-graphs/delete.mdx`

**Endpoint:** `DELETE /api/v1/assistant-graphs/{graph_id}`

**Content:**
- Description
- Path parameters
- Response
- Behavior (unassigns phone numbers)
- Example

---

### 7. `api-reference/assistant-graphs/nodes.mdx`

**Endpoints:** Node management

**Content:**
- `POST /api/v1/assistant-graphs/{graph_id}/nodes` - Add node
- `PUT /api/v1/assistant-graphs/{graph_id}/nodes/{node_id}` - Update node
- `DELETE /api/v1/assistant-graphs/{graph_id}/nodes/{node_id}` - Delete node
- Node schemas
- Node types explanation
- Examples

---

### 8. `api-reference/assistant-graphs/edges.mdx`

**Endpoints:** Edge management

**Content:**
- `POST /api/v1/assistant-graphs/{graph_id}/edges` - Add edge
- `PUT /api/v1/assistant-graphs/{graph_id}/edges/{edge_id}` - Update edge
- `DELETE /api/v1/assistant-graphs/{graph_id}/edges/{edge_id}` - Delete edge
- Edge schemas
- Condition types detailed documentation
- Priority explanation
- Examples

---

### 9. `api-reference/assistant-graphs/sessions.mdx`

**Endpoint:** `GET /api/v1/assistant-graphs/{graph_id}/sessions`

**Content:**
- Description: View conversation sessions that used this graph
- Query parameters (date filters, status)
- Response schema:
  - Session ID
  - Transitions made (which nodes were visited)
  - Intent detections
  - Duration per node
- Example

---

### 10. `api-reference/assistant-graphs/assign-phone.mdx`

**Endpoint:** `POST /api/v1/assistant-graphs/{graph_id}/phone-numbers`

**Content:**
- Description: Assign phone number to graph
- Request body (phone_number_id)
- Response
- Behavior (overrides individual assistant assignment)
- Example

---

## Navigation Updates

Add to `docs.json`:

**In Features group:**
```json
"assistant-graphs"
```

**New API Reference group:**
```json
{
  "group": "Assistant Graphs",
  "pages": [
    "api-reference/assistant-graphs/create",
    "api-reference/assistant-graphs/list",
    "api-reference/assistant-graphs/get",
    "api-reference/assistant-graphs/update",
    "api-reference/assistant-graphs/delete",
    "api-reference/assistant-graphs/nodes",
    "api-reference/assistant-graphs/edges",
    "api-reference/assistant-graphs/sessions",
    "api-reference/assistant-graphs/assign-phone"
  ]
}
```

---

## Implementation Steps

1. Create `mintlify-docs/api-reference/assistant-graphs/` directory
2. Create main `assistant-graphs.mdx` guide
3. Create all API reference pages
4. Update navigation in `docs.json`
5. Add cross-references from phone number docs
6. Add diagrams for graph visualization
7. Test all examples

---

## Source Files to Reference

- `app/api/assistant_graphs.py` - All API endpoints
- `app/api/schemas.py` - Request/response schemas (search for Graph*)
- `app/services/assistant_graph_service.py` - Business logic
- `app/db/models.py` - AssistantGraph, GraphNode, GraphEdge models
- `Features.MD` Section 8

---

## Key Schemas from Source

**GraphNode:**
- `id`, `assistant_id`, `node_type`, `position_x`, `position_y`, `config`

**GraphEdge:**
- `id`, `source_node_id`, `target_node_id`, `condition_type`, `condition_config`, `priority`, `handoff_sentence`

**Condition Types:**
- `intent` - Intent detection
- `natural_language` - LLM evaluation
- `mathematical` - Expression evaluation
- `state_variable` - Variable check

---

## Estimated Effort

- Main guide (assistant-graphs.mdx): 3 hours
- API reference pages (9 pages): 3 hours
- Navigation + diagrams: 1 hour
- Testing: 0.5 hours

**Total: ~7.5 hours**

---

## Dependencies

- None (can be implemented independently)

## Success Criteria

- Conceptual guide clearly explains multi-assistant orchestration
- All API endpoints documented with accurate schemas
- Condition types are well explained with examples
- Navigation correctly shows Assistant Graphs
- Examples work correctly
- Diagrams help visualize graph concepts
