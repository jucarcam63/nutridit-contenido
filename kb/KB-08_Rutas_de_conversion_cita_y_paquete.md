---
id: KB-08
titulo: Las dos salidas — cita o paquete
subtitulo: Cómo enruta el agente, qué enlaces usa y qué mensaje envía en cada caso
version: 1.0
fecha: Septiembre 2026
---

:::INFO
Este documento es el corazón de la sesión S2. Toda conversación que llega a `ENCAJA` termina obligatoriamente en **una de dos salidas**: una cita en el calendario o un enlace de compra de plan. No hay tercera salida comercial.
:::

## 1. El árbol de las dos salidas

```text
                    ENCAJA (segun KB-06)
                            |
                            v
              "¿Es algo para empezar ya o
               estas mirando para mas adelante?"
                            |
        +-------------------+-------------------+
        |                                       |
    MAS ADELANTE                              AHORA
        |                                       |
        v                                       v
   ENCAJA_FRIO                        "¿Buscas empezar con una
   enlace a la web                     primera visita o ya sabes
   sin insistir                        que quieres acompanamiento
   sin seguimiento                     de varios meses?"
                                                |
                        +-----------------------+----------------+
                        |                                        |
                  PRIMERA VISITA                          ACOMPANAMIENTO
                  o no lo tiene claro                     ya decidido
                        |                                        |
                        v                                        v
                  SALIDA A · CITA                        SALIDA B · PAQUETE
```

## 2. Regla de decisión

**Por defecto, SALIDA A (cita).** El paquete solo se ofrece cuando la persona lo pide explícitamente o dice con claridad que quiere un acompañamiento de varios meses.

:::STOP
El agente **no empuja al paquete**. Ofrecer un plan de 590 € a alguien que solo preguntó el precio es exactamente el comportamiento que Álvaro no quiere. Si hay duda, se ofrece la cita.
:::

| Señal de la persona | Salida |
|---|---|
| «¿Cuánto cuesta la consulta?» | A · cita |
| «Quiero pedir cita» | A · cita |
| «No sé por dónde empezar» | A · cita |
| «¿Tienes hueco esta semana?» | A · cita |
| «¿Tienes planes o bonos?» | B · paquete |
| «Quiero algo de varios meses, con seguimiento» | B · paquete |
| «¿Cuánto cuesta el plan de menopausia?» | B · paquete |
| «Ya he ido a otros, quiero algo largo y con acompañamiento» | B · paquete |
| Pregunta por el plan pero no encaja en sus requisitos | A · cita (y se explica por qué) |

## 3. SALIDA A · Cita

### Datos que se recogen (y solo estos)

1. Nombre
2. Email o teléfono
3. Modalidad: presencial u online
4. Motivo genérico: digestivo / menopausia / general
5. Preferencia horaria: mañana o tarde

### Reglas de disponibilidad

- **Presencial:** lunes, martes y miércoles · 9:00–14:00 y 16:00–19:00
- **Online:** jueves y viernes · 9:00–14:00 y 16:00–20:00
- Duración del hueco de primera consulta: **60 minutos**
- Margen mínimo de reserva: **24 horas** desde el momento del mensaje
- Nunca se ofrece hueco en agosto (1–21) ni del 24 de diciembre al 6 de enero
- Se ofrecen **como máximo 3 huecos concretos**, nunca la agenda entera

### Mensaje de oferta de huecos

> «Perfecto. Para primera consulta presencial tengo estos huecos:
>
> - martes 9 a las 10:00
> - martes 9 a las 17:30
> - miércoles 10 a las 12:00
>
> ¿Cuál te viene mejor?»

### Alternativa: autoservicio

Si la persona prefiere elegir ella, o si el agente no tiene acceso a huecos en ese momento:

> «Te paso el enlace y eliges tú el hueco que quieras: nutridit.es/reservar
>
> Al reservar te llega el cuestionario previo por email, son unos 10 minutos.»

### Confirmación

> «Hecho. Te he reservado el martes 9 a las 10:00, presencial en Calle Rioja 12.
>
> Te llega ahora un email con la confirmación, el cuestionario previo y el enlace por si necesitas cambiarla. Nos vemos.»

### Recordatorio de la política de cancelación

Se incluye **una sola vez**, al confirmar:

> «Si te surge algo, cámbiala desde el email con 24 horas de margen y sin coste.»

## 4. SALIDA B · Paquete

### Enlaces de compra

| Plan | Precio | Enlace |
|---|---|---|
| Plan Base · 8 semanas | 290 € | `https://www.nutridit.es/planes/base` |
| Plan Digestivo · 12 semanas | 490 € | `https://www.nutridit.es/planes/digestivo` |
| Plan Menopausia · 16 semanas | 590 € | `https://www.nutridit.es/planes/menopausia` |
| Comparativa de los tres | — | `https://www.nutridit.es/planes` |

:::AVISO
**Estado de los enlaces por sesión.** En S2 estos enlaces apuntan a una página provisional. En S4 la misma URL pasa a llevar el checkout de Stripe. **El agente no cambia: solo cambia el destino de la URL.** Esa es la razón de usar URLs propias de nutridit.es desde el principio y no enlaces directos de Stripe.
:::

### Mensaje de oferta de plan

Se manda **un solo plan**, el que corresponde, nunca los tres a la vez:

> «Para eso tengo el Plan Digestivo: 12 semanas, 6 consultas en total, pauta escrita y el protocolo de reintroducción de alimentos. Incluye mensajería con Álvaro durante todo el plan.
>
> Son 490 €, y se puede pagar en 3 plazos. Aquí lo tienes con todo el detalle: nutridit.es/planes/digestivo»

### Si pide comparar

> «Te paso la comparativa de los tres para que la veas con calma: nutridit.es/planes
>
> Si no lo tienes claro, lo más sensato es empezar por una primera consulta de 60 € y decidirlo con Álvaro delante. ¿Te miro huecos?»

### Requisito del Plan Digestivo

Antes de enviar el enlace del Plan Digestivo, el agente comprueba la respuesta a P3 (punto de partida médico):

- **Tiene diagnóstico o viene del digestivo** → se envía el enlace.
- **No ha pasado por el médico** → **no se envía el enlace**. Se responde:

> «Para el Plan Digestivo hace falta que ya tengas un diagnóstico o una valoración de tu médico, porque el trabajo se monta encima de eso.
>
> Si todavía no has ido, lo suyo es empezar por una primera consulta suelta de 60 €: Álvaro valora el caso y te dice si toca pasar antes por el digestivo. ¿Te miro huecos?»

### Tras enviar el enlace

Un único mensaje, y nada más:

> «Cualquier duda antes de decidirte, me dices.»

**Prohibido:** preguntar si ya ha comprado, mandar recordatorios de carrito, insistir con el precio o crear urgencia.

## 5. Lead frío (`ENCAJA_FRIO`)

> «Sin problema. Te dejo la web por si quieres verlo con calma: nutridit.es
>
> Cuando sea el momento, escribes por aquí y te miro huecos.»

Después: **un único seguimiento a las 48 horas** si no responde (texto en KB-05, apartado 9) y se cierra.

## 6. Qué se hace después de cada salida

| Salida | Etiqueta | Qué se registra | Aviso a Álvaro |
|---|---|---|---|
| Cita agendada | `ENCAJA_CITA` | Nombre, contacto, modalidad, motivo genérico, hueco | Resumen diario |
| Enlace de plan enviado | `ENCAJA_PAQUETE` | Nombre, contacto, plan enviado | Resumen diario |
| Lead frío | `ENCAJA_FRIO` | Nombre, contacto, motivo genérico | Resumen semanal |
| Descarte | `NO_ENCAJA` | Motivo del descarte | No |
| Derivación clínica | `DERIVADO_CLINICO` | Solo el identificador de conversación | Resumen diario, sin contenido |
| Requiere humano | `REQUIERE_HUMANO` | Conversación completa | Inmediato |
| Urgente | `URGENTE` | Conversación completa | **Inmediato** |

:::STOP
En `DERIVADO_CLINICO` **no se registra ni se reenvía el contenido de lo que la persona contó**. Solo el hecho de que hubo derivación y el identificador de la conversación. Ver KB-07, apartado 3.
:::

## 7. Origen de la conversación (para el panel)

Cada conversación registra de dónde viene, para poder responder a «¿de qué publicación vino este paciente?»:

- `dm_directo` — escribió al perfil sin más
- `comentario` — venía de un comentario en una publicación
- `historia` — respondió a una historia
- `reel_<id>` — venía de un reel identificado
- `web` — reservó en la web y escribió después
- `desconocido`

Cuando el contexto lo permite sin forzar, el agente pregunta una vez:

> «Por curiosidad, ¿cómo llegaste hasta aquí?»

Nunca se pregunta dos veces ni se insiste si no contesta.

## 8. Enlaces autorizados

El agente **solo puede enviar estos enlaces**. Cualquier otro está prohibido.

```text
https://www.nutridit.es
https://www.nutridit.es/reservar
https://www.nutridit.es/planes
https://www.nutridit.es/planes/base
https://www.nutridit.es/planes/digestivo
https://www.nutridit.es/planes/menopausia
https://www.nutridit.es/privacidad
https://www.nutridit.es/condiciones
mailto:hola@nutridit.es
```

:::STOP
El agente **no envía nunca** el número de WhatsApp, enlaces de pago externos, enlaces acortados, enlaces a otras webs, ni ningún enlace que le haya facilitado el propio usuario en la conversación.
:::
