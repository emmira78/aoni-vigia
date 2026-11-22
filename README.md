# Vigía del VPS de Aoni Teema

Si Hostinger apaga el servidor de aoniteema.com y no lo vuelve a encender, esto lo enciende solo.

Pasó el 16 y el 17/10/2026, durante el mantenimiento de emergencia de Hostinger por una falla de seguridad de Linux (CVE-2026-99999). Las dos veces el VPS quedó «Detenido». La segunda, la web estuvo caída 14 horas de noche, hasta que alguien lo encendió a mano.

## Qué hace

Cada 5 minutos (`.github/workflows/vigia.yml`):

1. Mira si `https://aoniteema.com/` responde. Si responde, no hace nada más.
2. Si no responde en tres intentos seguidos, le pregunta a Hostinger en qué estado está el VPS.
3. **Solo si está «stopped»**, lo enciende. En ese caso la corrida termina en rojo a propósito, para que GitHub mande un correo avisando.

Si el VPS figura encendido o suspendido, no toca nada: ahí el problema es otro, por ejemplo un despliegue en curso o un pago, y lo tiene que mirar una persona.

## Lo que necesita

Un secreto del repositorio llamado `MONITOREO_AONI`, con un token de la API de Hostinger (hPanel → API).

- Los secretos van cifrados y no aparecen nunca en el código ni en los registros.
- Una vez por día, en la primera corrida después de las 9 de Argentina, el vigía consulta el estado del VPS aunque el sitio ande. Si el token venció o se borró, esa corrida falla y GitHub manda un correo ese mismo día.
- GitHub no corre el horario cada 5 minutos: en noviembre de 2026 lo corrió de 3 a 5 veces por día. Por eso el vigía **no es un monitor de caídas**: enciende el VPS si Hostinger lo deja apagado, y nada más. Para enterarse de una caída o de una demora hace falta un monitor de afuera (UptimeRobot o parecido) con aviso al teléfono.
- Si el sitio no responde y el VPS figura encendido, la corrida termina en rojo (desde el 22/11/2026): así GitHub manda un correo. Antes dejaba un aviso amarillo y nadie se enteraba.

## Probarlo

En Actions → «Vigía del VPS de Aoni Teema» → **Run workflow**. Corrido a mano, siempre consulta el estado del VPS, así que sirve para confirmar que el token anda: el registro tiene que decir «El VPS está: running».

## Por qué es público y está aparte

Un repositorio público tiene minutos de GitHub Actions ilimitados. En el repositorio privado de Aoni Teema, una corrida cada 5 minutos se comería el tope gratis del mes y frenaría el CI.
