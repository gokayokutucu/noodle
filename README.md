

# Noodle

Interactive JSON-to-Graph visualizer and editor built with D3.js.
The purpose of this project is not only visualization but also to create relationships between different data models for AI/RAG. These relationships are exported as triples (subject–predicate–object) to feed into AI training or Retrieval-Augmented Generation (RAG) pipelines.

![demo](./screenshot.png)

## Features

- **Visualize JSON as a graph**  
  Models (white nodes) and their items (black nodes) are displayed.
- **Drag & Drop relationships**  
  Create edges by dragging an item onto a model; arrows are drawn automatically.
- **Live JSON editor & validation**  
  Left sidebar contains the underlying JSON; changes are reflected in the graph.
- **Schema validation**  
  JSON is checked against a schema (valid/invalid status shown).
- **Export as edge triples**  
  Export the graph as subject–predicate–object triples for AI / RAG pipelines.
- **Zoom & Pan**  
  - Hold `Space` to pan (hand cursor).  
  - Hold `Alt/Option` + scroll to zoom in/out.  
  - Use header buttons (+ / – / Fit) to control view.
- **Sidebar toggle**  
  Collapse/expand sidebar with ☰ button in header.

## Getting Started

### Run locally

```bash
git clone https://github.com/<your-username>/noodle.git
cd noodle
open index.html   # or just double-click in Finder/Explorer
```

No build step is required; everything runs directly in the browser.

### Editing

- Edit `index.html` for code.
- JSON data lives in the left sidebar editor; can be updated live.

## Usage

1. Start with the default JSON structure.
2. Drag items to models to create new mappings.
3. Watch edges update in the graph and JSON.
4. Export triples via the **Export** button.

Example triple output:


```json
{
  "triples": [
    { "s": "Apple.iPhone", "p": "mapsTo", "o": "Sony" },
    { "s": "Apple.iPad",   "p": "mapsTo", "o": "Samsung" },
    { "s": "Apple.iPhone", "p": "belongsTo", "o": "Apple" }
  ]
}
```

## Data Model & Schema

The graph is backed by a JSON document that describes models, items, and their links. The format is designed to be simple for humans to edit, while still validatable against a JSON Schema.

### Example JSON
```json
{
  "version": "1.0",
  "nodes": [
    { "id": "Apple", "type": "model", "label": "Apple" },
    { "id": "Apple.iPhone", "type": "item", "label": "iPhone", "of": "Apple", "links": [
      { "target": "Sony", "rel": "mapsTo" }
    ]},
    { "id": "Sony", "type": "model", "label": "Sony" }
  ]
}
```

### JSON Schema
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["version", "nodes"],
  "properties": {
    "version": { "type": "string" },
    "nodes": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["id", "type"],
        "additionalProperties": false,
        "properties": {
          "id": { "type": "string", "minLength": 1 },
          "type": { "type": "string", "enum": ["model", "item"] },
          "label": { "type": "string" },
          "of": { "type": "string" },
          "links": {
            "type": "array",
            "items": {
              "type": "object",
              "required": ["target", "rel"],
              "additionalProperties": false,
              "properties": {
                "target": { "type": "string", "minLength": 1 },
                "rel": { "type": "string", "minLength": 1 },
                "confidence": { "type": "number", "minimum": 0, "maximum": 1 },
                "note": { "type": "string" },
                "meta": { "type": "object", "additionalProperties": true },
                "createdAt": { "type": "string", "format": "date-time" },
                "createdBy": { "type": "string" }
              }
            }
          }
        }
      }
    }
  }
}
```

This schema ensures that nodes and their relationships are well-formed and suitable for use in downstream AI/RAG pipelines.

## Roadmap

- [ ] Edge deletion support  
- [ ] Curved edges for readability  
- [ ] Import multiple JSON files  
- [ ] Save/load graph state

## License

Apache License