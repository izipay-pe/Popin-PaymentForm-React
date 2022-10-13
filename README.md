# Popin Payment Form React

Esta página explica cómo crear un formulario de pago dinámico desde cero utilizando React y la biblioteca de embedded-form-glue.

<a name="Requisitos_Previos"></a>

## Requisitos Previos

* Extraer credenciales del Back Office Vendedor. [Guía Aquí](https://github.com/izipay-pe/obtener-credenciales-de-conexion)
* Debe instalar la [versión de LTS node.js](https://nodejs.org/es/).

## 1.- Crear el proyecto 
* Descargar el proyecto .zip ingresando [aquí](https://github.com/izipay-pe/Popin-PaymentFormT1-React/archive/refs/heads/main.zip) ó clonarlo con git.
```sh
git clone https://github.com/izipay-pe/Popin-PaymentFormT1-React.git
``` 

* Ingrese a la carpeta raiz del proyecto.

* Agregue la dependencia embedded-form-glue o instale todas las dependencias que necesita el proyecto:

```sh
npm install --save @lyracom/embedded-form-glue
ó
npm install
```

* Ejecútelo y pruébelo:
```sh
npm start
```

ver el resultado en http://localhost:3000/

## 2.- Agregar el formulario de pago
**Nota**: Reemplace **[CHANGE_ME]** con sus credenciales de `API REST` extraídas desde el Back Office Vendedor, ver [Requisitos Previos](#Requisitos_Previos).

* Editar en public/index.html en la sección HEAD.

```javascript
<!-- tema y plugins. debe cargarse en la sección HEAD -->
<link rel="stylesheet"
href="~~CHANGE_ME_ENDPOINT~~/static/js/krypton-client/V4.0/ext/classic-reset.css">
<script
    src="~~CHANGE_ME_ENDPOINT~~/static/js/krypton-client/V4.0/ext/classic.js">
</script>
```

* Edite el componente predeterminado src/App.js, con el siguiente codigo si quiere interactuar con el formulario de pago, con un endpoint propio.

```javascript
import { useState } from 'react';
import KRGlue from '@lyracom/embedded-form-glue';
import axios from 'axios';
import PaymentForm from './components/PaymentForm';

function App() {

  const [isShow, setIsShow] = useState(false);
  const [isValid, setIsValid] = useState(true);
  const [amount, setAmount] = useState("");

  const publicKey = "~~CHANGE_ME_ENDPOINT~~";
  const endPoint = "~~CHANGE_ME_ENDPOINT~~";
  const formToken = "~~CHANGE_ME_ENDPOINT~~";
  const server = "http://localshot:3000";
  
  const payment = ()=>{
    const ExpRegSoloNumeros="^[0-9]+$";
    if(amount.match(ExpRegSoloNumeros)!=null){
      // Obtener el formToken
      getFormToken(amount,publicKey,endPoint);
      setIsShow(true);
    }else{
      setIsValid(false);
      setTimeout(()=>setIsValid(true),3000)
    }
  }
  const getFormToken = (monto, publicKey, domain) => {
    const dataPayment = {
        amount: monto*100,
        currency: "USD",
        customer:{
          email: "example@gmail.com"
        },
        orderId: "pedido-0"
    }
    KRGlue.loadLibrary(domain,publicKey)
    .then(({KR}) => KR.setFormConfig({
    formToken: formToken
    }))
    .then(({ KR }) => KR.onSubmit(validatePayment) )
    .then(({ KR }) => KR.attachForm("#form") )
    .then(({ KR, result }) => KR.showForm(result.formId))
    .catch(err=>console.log(err))

  }

  const validatePayment = (resp) => {
    axios.post(`${server}/api/validatePayment`, resp)
    .then(({data}) => {
      if (data==="Valid Payment"){
        setIsShow(false);
        alert("Pago Satisfactorio");
        
      }else{
        alert("Pago Inválido");
      }
    })
    return false;
  }

  return (
    <div className='container'>
        <main>
            <div className="py-5 text-center">      
                ...   
            </div>

            <div className="row g-5">
                <div className="col-md-5 col-lg-4 order-md-last">
                    <div className="d-flex justify-content-center">
                        <div id="myDIV" className="formulario" style={{display: isShow?"block":"none" }}>
                            <div id="form">
                                <PaymentForm popin={true} />
                            </div> 
                        </div>                         
                    </div>                    
                    <hr className="my-4"/>
                </div>

                <div className="col-md-7 col-lg-8">          
                    <form className="needs-validation">
                        ...   
                        <button onClick={payment} className="w-100 btn btn-primary btn-lg" type="button">Finalizar con el Pago</button>
                    </form>
                </div>
            </div>
        </main>
    </div>
  );
}
export default App;
```
## 5.- Implementar IPN

* Ver manual de implementacion de la IPN [Aquí](https://secure.micuentaweb.pe/doc/es-PE/rest/V4.0/kb/payment_done.html)

* Ver el ejemplo de la respuesta IPN [Aquí](https://github.com/izipay-pe/Redirect-PaymentForm-IpnT1-PHP)
