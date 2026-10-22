# Respuestas del Parcial 2

## P1. Regresión

La regla que no tenía cobertura era el tope: ningún ticket paga más de Q60. Agregué dos casos en bruno/regresion. Cada uno abre un ticket y luego lo cobra.

| Caso | Minutos | Sin tope daría | Resultado esperado según la tabla |
|---|---|---|---|
| 14-cobro-tope-seis-horas-y-fraccion | 361 (valor frontera, una fracción después de 6 horas) | Q70 | Q60 |
| 16-cobro-tope-diez-horas | 600 | Q100 | Q60 |

## P2. Las tres fallas del Jenkinsfile

1. El bru run de la regresión terminaba con || true, así que el paso siempre salía con código 0, la regresión nunca ponía el build en rojo y no funcionaba como puerta.
2. La etapa Build image corría antes que Regresion, así que la imagen del API se construía aunque la regresión fallara; ahora va después y se salta cuando la regresión falla.
3. junit leía bruno/smoke/results.xml, que este pipeline nunca genera, y con allowEmptyResults el build no reportaba ninguna prueba; ahora lee bruno/regresion/results.xml.

## P3. Evidencia

Job parcial2 en Jenkins, de tipo Pipeline, que lee el Jenkinsfile de este repositorio en la rama main.

| Paso | Build | SHA | Resultado |
|---|---|---|---|
| 1. Jenkinsfile corregido, sin la prueba de P1 | #3 | 1731554ba72a3cb389ad3db67a4e4717c7872fbe | Verde |
| 2. Prueba de P1 | #4 | 2524d1f5a1b12e13e117ad84364b400fa1fa1b91 | Rojo |
| 3. api.py cumple el tope | #5 | 8dc0125a6f5077a0a367aacef82061a0f11654d0 | Verde |

Los builds #1 y #2 del job son de un repositorio anterior que borré y volví a crear desde el template, así que no cuentan.

Aserción que falló en el build rojo (#4), tal como aparece en su Test Result: res.body.monto eq 60. Falló en 14-cobro-tope-seis-horas-y-fraccion y en 16-cobro-tope-diez-horas.

Capturas de la vista de Stages: evidencia/jenkins/01-rojo.png (build #4) y evidencia/jenkins/02-verde.png (build #5).
