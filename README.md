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

Un secreto del repositorio llamado `HOSTINGER_API_TOKEN`, con un token de la API de Hostinger (hPanel → API).

- Los secretos van cifrados y no aparecen nunca en el código ni en los registros.
- Una vez por día, a las 9 de Argentina, el vigía consulta el estado del VPS aunque el sitio ande. Si el token venció o se borró, esa corrida falla y GitHub manda un correo ese mismo día.

## Probarlo

En Actions → «Vigía del VPS de Aoni Teema» → **Run workflow**. Corrido a mano, siempre consulta el estado del VPS, así que sirve para confirmar que el token anda: el registro tiene que decir «El VPS está: running».

## Por qué es público y está aparte

Un repositorio público tiene minutos de GitHub Actions ilimitados. En el repositorio privado de Aoni Teema, una corrida cada 5 minutos se comería el tope gratis del mes y frenaría el CI.
