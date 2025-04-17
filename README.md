Integración con Mercado Pago — React + Java

Esta falopeada permite implementar pagos online utilizando el SDK oficial de **Mercado Pago**, tanto del lado del cliente (frontend con React) como del servidor (backend en Java) Y ta gueno para recibir transferencias.

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
// # Crear proyecto si no existe
npx create-react-app frontend --template typescript

cd frontend

# Instalar SDK Mercado Pago
npm install @mercadopago/sdk-react
Inicializar SDK

En la App.tsx o archivo principal:


import { initMercadoPago } from '@mercadopago/sdk-react';

initMercadoPago('TU_PUBLIC_KEY'); // se obtiene desde el panel de Mercado Pago
Usar el botón de pago

import { Wallet } from '@mercadopago/sdk-react';

<Wallet initialization={{ preferenceId: '<PREFERENCE_ID>' }} />

//
## Backend (Java + Spring Boot)
```bash
Agregá la dependencia en tu pom.xml:


<dependency>
  <groupId>com.mercadopago</groupId>
  <artifactId>dx-java</artifactId>
  <version>2.1.10</version>
</dependency>
Crea una preferencia de pago

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
