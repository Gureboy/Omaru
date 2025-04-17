# Omaru
Solo para prueba falopa
Implementar una solución completa de pagos y cobros utilizando el SDK oficial de Mercado Pago, permitiendo generar transferencias, recibir pagos, y gestionar preferencias desde una aplicación web moderna.

🛠️ Stack recomendado
Frontend (Cliente):
✅ React — recomendado oficialmente por Mercado Pago
🟨 También compatibles: Vue.js, Angular (pero requieren wrappers personalizados o adaptadores)
❌ No se recomienda mezclar frameworks en un mismo proyecto, salvo en microfrontends muy avanzados.

Backend (Servidor):
✅ Java (Spring Boot) — completamente compatible y robusto
🟩 También posibles: Node.js, Python, PHP, etc.
⚠️ Mercado Pago provee SDKs oficiales para backend en Java: mercadopago Java SDK

📦 Instalación del SDK (Cliente)

npm install @mercadopago/sdk-react
🧠 Inicialización del SDK en React

// src/App.tsx o App.jsx
import { initMercadoPago } from '@mercadopago/sdk-react';

initMercadoPago('TU_PUBLIC_KEY'); 
💳 Integración del Botón de Pago


import { Wallet } from '@mercadopago/sdk-react';

<Wallet initialization={{ preferenceId: '<PREFERENCE_ID>' }} />
⚠️ El preferenceId se obtiene desde el backend al crear una preferencia de pago (ver más abajo).

🖥️ Backend en Java (Spring Boot) — Crear una Preferencia
java

import com.mercadopago.MercadoPago;
import com.mercadopago.exceptions.MPException;
import com.mercadopago.resources.Preference;
import com.mercadopago.resources.datastructures.preference.*;

@PostMapping("/create_preference")
public ResponseEntity<?> createPreference() {
    MercadoPago.SDK.setAccessToken("ACCESS_TOKEN");

    Preference preference = new Preference();

    Item item = new Item()
        .setTitle("Servicio/Producto")
        .setQuantity(1)
        .setUnitPrice((float) 1500.00);

    preference.appendItem(item);

    try {
        preference.save();
        return ResponseEntity.ok(Map.of("preferenceId", preference.getId()));
    } catch (MPException e) {
        return ResponseEntity.status(500).body("Error: " + e.getMessage());
    }
}
🔐 Credenciales
Public Key: usada en el frontend

Access Token: usada en el backend

Las obtenés desde Mercado Pago Developers > Credenciales

✅ Flujo resumido de integración

graph TD;
  A[Usuario en React] -->|Click en botón| B[Componente Wallet]
  B -->|Usa preferenceId| C[Renderiza botón de pago]
  C -->|Hace pago| D[Mercado Pago]
  D -->|Notifica| E[Backend Java con Webhook]
  E -->|Guarda datos| F[Base de datos o dashboard]
📡 Webhooks para notificaciones de pago
Mercado Pago puede enviarte una notificación automática cuando se realiza un pago. Debes configurar una URL pública en:

Configuración de Webhooks




@PostMapping("/webhook")
public ResponseEntity<?> receiveWebhook(@RequestBody String payload) {
    // Procesar evento
    System.out.println("Pago recibido: " + payload);
    return ResponseEntity.ok().build();
}
📂 Estructura recomendada del proyecto

/mercadopago-react-app
│
├── frontend/
│   ├── src/
│   │   ├── App.jsx
│   │   └── components/
│   │       └── PaymentButton.jsx
│   └── package.json
│
└── backend/
    ├── src/main/java/com/tuproject/
    │   ├── controller/PaymentController.java
    │   └── config/WebhookController.java
    └── pom.xml
