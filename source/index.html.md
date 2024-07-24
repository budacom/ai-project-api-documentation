---
title: API Reference

language_tabs:
  - javascript

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for Project AI API
---

# API del Proyecto IA 🤖

Bienvenidos a la documentación de la API del proyecto IA.
Su función es conectar patabit con assistants de Open AI para revisar verificaciones avanzadas.

## Post to check verification

```javascript
const data = {
  names: "JUAN ANDRÉS",
  surnames: "CASTILLO PEREZ",
  residence_address: "Cristóbal Colón 1102",
  residence_country: "CL",
  residence_state: "Metropolitana",
  residence_comune: "Las Condes",
  residence_city: "Santiago",
  public_url: "https://prc-surbtc-patabit.s3.amazonaws.com/surbtc_documents/aaa.pdf?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=a/us-east-1/s3/aws4_request&X-Amz-Date=a&X-Amz-SignedHeaders=host&X-Amz-Signature=aa",
  liveness_check: true
};

const options = {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(data)
};

const url = 'https://api.example.com/endpoint';

fetch(url, options)
  .then(response => response.json())
  .then(data => {
    console.log('Success:', data);
  });
```

> Esta llamada retorna un JSON con el siguiente formato:

```json
{
  "name_validator": {
    "name_in_document": "JUAN ANDRÉS CASTILLO",
    "match_score": 100,
    "veredict": "approved",
    "reasons": [],
  },
  "address_validator": {
    "address_in_document": "Cristóbal Colón 1102, Santiago",
    "match_score": 75,
    "veredict": "need_review",
    "reasons": "No aparece la comuna. Se muestran diferencias en algunas caracteres.",
  },
  "document_completeness": {
    "veredict": "rejected",
    "reasons": "No se ve la esquina izquierda",
  },
  "document_emission_date_validator": {
    "emission_date": "2024-07-24",
    "veredict": "approved",
    "reasons": [],
  },
  "code_validator": {
    "document_code": true,
    "veredict": "approved",
    "reasons": [],
  },
  "final_veredict": "rejected",
  "full_reasons": ["No aparece la comuna. Se muestran diferencias en algunas caracteres.", "No se ve la esquina izquierda"],
}
```

Esta llamada permite mandar los datos de una verificación avanzada para que sea revisada por los assistants de Open AI.

### HTTP Request

`POST /check-verification`

### Request Payload

Key | Required | Description
--------- | ------- | -----------
names | Yes | Nombre del usuario
surnames | Yes | Apellidos del usuario
residence_address | Yes | Dirección del usuario
residence_country | Yes | País de residencia del usuario
residence_state | Yes | Estado donde reside el usuario
residence_comune | Yes | Comuna donde reside el usuario
residence_city | Yes | Ciudad de residencia del usuario
public_url | Yes | Link al documento de residencia
liveness_check | Yes | Indica si la cuenta tiene prueba de vida o no

### General Response Details

Key | Tipo | Description
--------- | ------- | -----------
name_validator | {object} | Resultado de la revisión del assistant que valida el nombre del usuario
address_validator | {object} | Resultado de la revisión del assistant que valida la dirección del usuario
document_completeness | {object} | Resultado de la revisión del assistant que verifica que el documento no esté recortado
document_emission_date_validator | {object} | Resultado de la revisión del assistant que verifica que extrae la fecha de emisión del documento
code_validator | {object} | Resultado de la revisión del assistant que verifica que el documento contenga firma, sello, QR, número de referencia
final_veredict | string | Veredicto final en base a la revisión de cada assistant. Indica si la verificaición se debe rechazar o aprobar
full_reasons | [array] | Revisión del assistant que valida el nombre del usuario
