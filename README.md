# Popin Payment Form React
Este es un ejemplo practico con la pasarela de pago de Izipay utilizando el formulario de pago popin.  
Visite la documentación para más información aquí: [Documentación Izipay](https://secure.micuentaweb.pe/doc/es-PE/rest/V4.0/javascript/quick_start_popin.html)

## Requisitos Previos
* Tener nodejs version 14.0.0 o posteriores.
* Servidor Backend. Ejemplo de servidor en nodejs [aquí]()
* Claves de integración. [Guía de como obtenerlo]()

## 1.- Descargar el proyecto 
Descargar el proyecto .zip ingresado [aquí](https://github.com/izipay-pe/Popin-PaymentFormT1-React/archive/refs/heads/main.zip) ó clonarlo con git.

```sh
git clone https://github.com/izipay-pe/Popin-PaymentFormT1-React.git
``` 

## 2.- Configurar las credenciales (Frontend):
Obtener las credenciales de su Back Office Vendedor y copiar la `Clave Pública` en las variable correspondiente en el archivo creado llamado: `App.js` 

```sh
    const publicKey = "{ShopId}:testpublickey_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX";
    const endPoint = "https://api.micuentaweb.pe";
    const server = "http://yourserver.pe";
``` 

## 2.- Configurar las credenciales (Backend):
Obtener las credenciales de su Back Office Vendedor y copiar las variables correspondientes en el archivo de configuración de su backend

```sh
    ID_STORE=*********** Identifier of the store of your merchant
    TEST_PASSWORD=************** Test password of your store
    PROD_PASSWORD=************** PROD password of your store
    TEST_KEY_HMAC_SHA_256=****************** HMAC TEST key of your trade
    PROD_KEY_H,AC_SHA_256=****************** HMAC PROD key of your trade
```

## 3.-Configurar la respuesta del pago por IPN 
Configurar la URL de notificación al final del pago para que su servidor web esté al tanto de la información del estado de pago de la transacción. Vea la documentación para más información. Aquí [IPN](https://secure.micuentaweb.pe/doc/es-PE/form-payment/quick-start-guide/implementar-la-ipn.html)  

Ejemplo de implementación. [Aquí](https://github.com/izipay-pe/Redirect-PaymentForm-IpnT1-PHP#redirect-paymentform-ipnt1-php)

## 4- Ejecutar el proyecto con el siguiente comando 
```sh
npm install
npm start
```

## 5.- Demo
Url de ejemplo [Formulario Popin](https://app-izipay.000webhostapp.com/popin/)