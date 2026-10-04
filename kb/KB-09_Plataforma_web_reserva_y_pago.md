---
id: KB-09
titulo: Plataforma web, reserva y pago
subtitulo: Estructura de nutridit.es, formulario de reserva, checkout y condiciones de venta
version: 1.0
fecha: Septiembre 2026
---

:::INFO
Este documento cubre las sesiones S3 y S4: la plataforma donde aterrizan las dos salidas del agente y donde se cobra. El agente lo necesita para responder dudas sobre la reserva y el pago **sin inventarse cómo funciona la web**.
:::

## 1. Mapa de la plataforma

```text
nutridit.es
   |
   +-- /                      Escaparate: a quien ayuda y que trata
   +-- /sobre-alvaro          Quien es, colegiacion, formacion
   +-- /consulta              Como funciona la primera consulta y precios sueltos
   +-- /planes                Comparativa de los tres planes
   |      +-- /planes/base
   |      +-- /planes/digestivo
   |      +-- /planes/menopausia
   +-- /reservar              Formulario de reserva de primera consulta
   +-- /cuestionario          Cuestionario previo (solo con enlace, tras reservar)
   +-- /gracias               Confirmacion de reserva
   +-- /compra-confirmada     Confirmacion de pago
   +-- /condiciones           Condiciones de venta e informacion precontractual
   +-- /privacidad            Politica de privacidad e informacion de tratamiento
   +-- /aviso-legal
   +-- /panel                 [S8] Espacio reservado, acceso privado
```

## 2. Qué dice la portada

**Lo primero, en la parte alta:** a quién ayuda y qué trata. No los títulos, no el precio en grande, no las redes sociales.

> **Nutrición clínica en Sevilla y online**
> Salud digestiva y acompañamiento nutricional de la menopausia.
> Álvaro Ferrer, dietista-nutricionista colegiado. 12 años de consulta.
>
> [Reservar primera consulta — 60 €]

**Debajo, por orden:** qué trata (las tres áreas) · cómo funciona la primera consulta · los tres planes · quién es Álvaro con su colegiación · preguntas frecuentes · el espacio del panel (vacío hasta S8).

**Lo que la portada no lleva:** fotos de antes y después, testimonios de pacientes sobre resultados de salud, contadores de kilos perdidos, cifras de eficacia, ni ningún banner de urgencia.

## 3. Formulario de reserva (`/reservar`)

### Campos permitidos

| Campo | Obligatorio | Tipo |
|---|---|---|
| Nombre y apellidos | Sí | Texto |
| Email | Sí | Email |
| Teléfono | No | Teléfono |
| Modalidad | Sí | Presencial / Online |
| Motivo genérico | Sí | Digestivo / Menopausia / General |
| Preferencia horaria | No | Mañana / Tarde |
| ¿Cómo nos has conocido? | No | Desplegable |
| Consentimiento de tratamiento de datos | Sí | Casilla **sin premarcar** |

:::STOP
**Campos prohibidos en el formulario público**

- «Cuéntame tu caso» / «Describe tus síntomas» — campo de texto libre clínico
- Peso, talla, IMC
- Diagnósticos, patologías, medicación
- Subida de analíticas o informes
- Fecha de nacimiento completa (basta con mayor de 16, si acaso)
- DNI, número de la Seguridad Social, dirección postal
- Datos de tarjeta (los recoge Stripe, nunca la web)

Todo dato clínico se recoge **después**, en el cuestionario previo con enlace privado, o directamente en consulta.
:::

### Consentimiento

Casilla **sin marcar**, con el texto visible junto a ella (no escondido en el pie):

> ☐ He leído y acepto la **información sobre el tratamiento de mis datos**. Nutridit (Álvaro Ferrer Ossorio, NIF 28914673K) tratará mis datos para gestionar mi cita, sobre la base de la ejecución de un contrato o medidas precontractuales. No se ceden a terceros salvo obligación legal. Puedo ejercer mis derechos de acceso, rectificación, supresión, oposición, limitación y portabilidad escribiendo a hola@nutridit.es. Más información en la [política de privacidad](https://www.nutridit.es/privacidad).

Casilla **separada y también sin marcar** para comunicaciones comerciales, si se quisiera enviar contenido:

> ☐ Quiero recibir contenido y novedades de Nutridit por email. (Opcional; puedo darme de baja cuando quiera.)

:::AVISO
Las dos casillas van **separadas**. Condicionar la reserva a aceptar el email comercial no es válido.
:::

## 4. Cuestionario previo (`/cuestionario`)

- Se envía **por email tras reservar**, con enlace privado. No es público ni indexable.
- Aquí sí se recogen datos de salud, con su base jurídica: **consentimiento explícito del interesado** (RGPD art. 9.2.a) y finalidad de asistencia sanitaria (art. 9.2.h).
- Recoge: hábitos alimentarios, horarios, actividad, historia dietética, motivo de consulta, antecedentes relevantes y medicación.
- Es **opcional**: quien no lo rellene puede venir igual, se hace en la consulta.
- El aviso al inicio: «Esto lo lee únicamente Álvaro. No se comparte con nadie.»

## 5. Pago con Stripe

### Dónde vive cada cosa

```text
NUTRIDIT.ES                          STRIPE
-----------                          ------
Muestra el plan                      Formulario de tarjeta
Muestra precio y condiciones         Procesa el cobro
Envia al checkout           --->     Guarda los datos de pago
Recibe la confirmacion      <---     Devuelve OK / KO
Muestra /compra-confirmada           Emite el recibo

Los datos de la tarjeta NUNCA pasan por nutridit.es.
```

**Respuesta del agente a «¿es seguro pagar ahí?»:**

> «El pago lo procesa Stripe, que es la pasarela que usan muchas tiendas y servicios. Los datos de tu tarjeta no pasan por la web de Álvaro ni los ve él en ningún momento.»

### Flujo de compra

```text
Enlace del agente o de la web
        |
        v
  /planes/<plan>    (que incluye, que no, precio, condiciones)
        |
        v
  Boton "Contratar"
        |
        v
  Stripe Checkout   (email + tarjeta)
        |
        v
  /compra-confirmada
        |
        +--> Email de confirmacion con la factura
        +--> Email con el enlace para reservar la primera consulta
```

:::AVISO
**Modo test.** Durante las clases todo se prueba en **modo test de Stripe**. Nadie cobra dinero real. Tarjeta de prueba: `4242 4242 4242 4242`, cualquier fecha futura y cualquier CVC. En modo test, Stripe muestra un aviso visible de que no es un cobro real.
:::

## 6. Información precontractual

Lo que el cliente **tiene que ver antes de pulsar «pagar»**, en la propia página del plan:

1. **Identidad del prestador:** Álvaro Ferrer Ossorio, NIF 28914673K, Calle Rioja 12, 41001 Sevilla, hola@nutridit.es, colegiado AND-01847.
2. **Descripción del servicio:** número de consultas, duración de cada una, duración total del plan, qué material se entrega, qué incluye y qué no.
3. **Precio total final**, con impuestos incluidos. Sin costes ocultos.
4. **Forma y momento del pago:** pago único o 3 plazos.
5. **Duración del contrato** y ausencia de permanencia o renovación automática.
6. **Derecho de desistimiento:** plazo, cómo ejercerlo y formulario.
7. **Condiciones de cancelación** de sesiones individuales.
8. **Caducidad de las sesiones** no consumidas.
9. **Vía de reclamación:** hola@nutridit.es y hojas de reclamaciones oficiales disponibles en la consulta.

Base: arts. 97 y 60 del TRLGDCU (RD Legislativo 1/2007) y Ley 34/2002 de servicios de la sociedad de la información.

## 7. Derecho de desistimiento

**Plazo:** 14 días naturales desde la contratación.

**Cómo se ejerce:** enviando un email a hola@nutridit.es, o con el formulario disponible en `/condiciones`. No hace falta justificar el motivo.

**Devolución:** en un máximo de 14 días desde que se comunica, por el mismo medio de pago.

**Servicio ya iniciado:** si el cliente ha pedido expresamente empezar antes de que terminen los 14 días y ya ha consumido alguna sesión, se descuenta la parte proporcional de lo prestado (art. 108.3 TRLGDCU). El resto se devuelve.

**Consultas sueltas:** una consulta ya realizada no es reembolsable. Si aún no se ha realizado, se puede cancelar con las condiciones habituales.

**Respuesta del agente:**

> «Tienes 14 días naturales desde que lo contratas para echarte atrás sin dar explicaciones, escribiendo a hola@nutridit.es. Si ya habías hecho alguna consulta, se descuenta esa parte y se te devuelve el resto.»

## 8. Facturación

- La factura se genera automáticamente y se envía por email tras cada pago.
- Se puede pedir factura a nombre de empresa indicándolo antes del pago o escribiendo a hola@nutridit.es.
- Servicio exento de IVA (art. 20.1.3º LIVA): en la factura consta la exención.
- Duplicados: se piden por email.

## 9. El hueco del panel (`/panel`)

Espacio reservado en la plataforma, ya maquetado desde S4 aunque esté vacío hasta S8.

- **Ubicación:** ruta propia `/panel`, enlazada solo desde el pie y no desde la navegación principal.
- **Acceso:** privado. Solo Álvaro.
- **Formato previsto:** contenedor embebido a ancho completo, con relación de aspecto responsive.
- **Qué mostrará:** citas agendadas por semana, origen de cada una, contenido publicado y coste por llamada agendada.
- **El agente no habla del panel con nadie.** Si alguien pregunta por `/panel`, responde que es una zona privada de gestión.

## 10. Dudas frecuentes sobre la web y el pago

| Pregunta | Respuesta |
|---|---|
| «No me llega el email de confirmación» | Mira en spam y en promociones. Si no está, escribe a hola@nutridit.es y lo reenviamos. |
| «Me ha fallado el pago» | Vuelve a intentarlo desde el mismo enlace. Si sigue fallando, es cosa de tu banco: escríbenos a hola@nutridit.es y lo vemos. |
| «¿Puedo pagar por transferencia?» | Sí, para planes. Escribe a hola@nutridit.es y te pasamos los datos. |
| «¿Guardáis mi tarjeta?» | No. Los datos de la tarjeta los gestiona Stripe; Nutridit no los ve nunca. |
| «Quiero que borréis mis datos» | Se escribe a hola@nutridit.es y se atiende. **El agente etiqueta `REQUIERE_HUMANO` y avisa a Álvaro.** |
| «¿La web es de Álvaro?» | Sí, nutridit.es es la web oficial. Cualquier enlace de pago que no empiece por nutridit.es no es nuestro. |
