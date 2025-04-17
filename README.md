# 💳 Integración con Mercado Pago — React + Java

Este proyecto permite implementar pagos online utilizando el SDK oficial de **Mercado Pago**, tanto del lado del cliente (frontend con React) como del servidor (backend en Java). Ideal para plataformas de servicios, comercio electrónico o sistemas que necesiten cobrar o recibir transferencias.

---

## 📦 Tecnologías utilizadas

| Parte        | Tecnología              |
|--------------|--------------------------|
| Frontend     | React + SDK MercadoPago |
| Backend      | Java (Spring Boot)      |
| API Pagos    | Mercado Pago REST API   |

---

## 🚀 Instalación rápida

### 🖥️ Frontend (React)

```bash
# Crear proyecto si no existe
npx create-react-app frontend --template typescript

cd frontend

# Instalar SDK Mercado Pago
npm install @mercadopago/sdk-react
Inicializar SDK
En tu App.tsx o archivo principal:

tsx
Copiar
Editar
import { initMercadoPago } from '@mercadopago/sdk-react';

initMercadoPago('TU_PUBLIC_KEY'); // obtené desde el panel de Mercado Pago
Usar el botón de pago
tsx
Copiar
Editar
import { Wallet } from '@mercadopago/sdk-react';

<Wallet initialization={{ preferenceId: '<PREFERENCE_ID>' }} />
🔧 Backend (Java + Spring Boot)
xml
Copiar
Editar
<!-- pom.xml -->
<dependency>
  <groupId>com.mercadopago</groupId>
  <artifactId>dx-java</artifactId>
  <version>2.1.10</version>
</dependency>
Crear una preferencia de pago
java
Copiar
Editar
@PostMapping("/create_preference")
public ResponseEntity<?> createPreference() {
    MercadoPago.SDK.setAccessToken("ACCESS_TOKEN");

    Preference preference = new Preference();

    Item item = new Item()
        .setTitle("Producto o Servicio")
        .setQuantity(1)
        .setUnitPrice((float) 1500.00);

    preference.appendItem(item);
    preference.save();

    return ResponseEntity.ok(Map.of("preferenceId", preference.getId()));
}
🔐 Credenciales necesarias
Obtené tus claves desde el Panel de Desarrolladores de Mercado Pago:

Public Key → uso en el frontend

Access Token → uso en el backend

📡 Webhooks (opcional pero recomendado)
Mercado Pago puede enviarte notificaciones automáticas cuando se completa un pago.

Configuración
Ingresá a: Configuración Webhooks

Agregá tu URL pública: https://tu-dominio.com/webhook

Controlador de ejemplo en Java
java
Copiar
Editar
@PostMapping("/webhook")
public ResponseEntity<?> recibirWebhook(@RequestBody String payload) {
    System.out.println("Evento recibido: " + payload);
    return ResponseEntity.ok().build();
}
🗂️ Estructura del proyecto sugerida
bash
Copiar
Editar
mercadopago-integration/
├── frontend/                # App React con SDK Mercado Pago
│   └── src/
│       └── components/
│           └── PaymentButton.tsx
├── backend/                 # Spring Boot con controlador de pagos
│   └── src/main/java/com/tuproject/
│       └── controller/
│           ├── PaymentController.java
│           └── WebhookController.java
└── README.md
💡 Recomendaciones
Usar entornos .env o application.properties para proteger credenciales

Crear una base de datos para registrar pagos, usuarios, etc.

Asegurar la comunicación HTTPS en producción


