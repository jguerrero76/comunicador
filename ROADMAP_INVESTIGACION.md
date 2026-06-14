# Comunicador: Roadmap Basado en Investigación de Alargamiento de Frases

**Documento de traducción**: desde el informe "Cómo fomentar la creación de frases cada vez más largas en personas autistas" hacia cambios concretos en Comunicador.

---

## ✅ YA IMPLEMENTADO (alineado con investigación)

- **Niveles de ayuda escalonados** (1-3) → andamiaje desvanecible ✓
- **Vocabulario nuclear** (verbos, conectores, emociones) → 75-80% de comunicación ✓
- **Apoyos visuales** (emojis + pictogramas ARASAAC) ✓
- **Marcos de frase en Nivel 1** (starters predefinidos) ✓
- **Recast automático** parcial (sugerencias contextuales) ✓
- **Refuerzo natural integrado** (estrellas, gameplay) ✓
- **Previsibilidad y estructura** (pantallas, transiciones claras) ✓
- **Bajo ruido sensorial** (diseño minimalista, animaciones suaves) ✓

---

## 🔴 FALTANTE CRÍTICA 1: Detección de Perfil (Analítico vs. Gestalt)

### Problema actual
La app asume un perfil de procesamiento analítico "universal", pero la investigación es clara: **usuarios gestalt necesitan estrategia radicalmente diferente**. La ecolalia no es un error a eliminar; es una etapa funcional del NLA (Natural Language Acquisition).

### Cambio requerido
**Módulo de onboarding mejorado** (1-3 minutos, responden familia/logopeda):

```
1. ¿Nivel expresivo actual del usuario?
   ☐ No verbal o mínimamente verbal
   ☐ Palabras sueltas (1-2)
   ☐ Frases cortas de 2-3 palabras
   ☐ Frases más completas (4+ palabras)

2. ¿Cómo adquiere el lenguaje?
   ☐ Analítico: palabra suelta → combinaciones → frases
   ☐ Gestalt: frases completas memorizadas → mezcla/recombinación → palabras
   ☐ Mixto

3. Muestras de lenguaje actual (3 enunciados típicos):
   ___________
   ___________
   ___________
   (la app estima LME de base)

4. Procesador gestalt: ¿usa ecolalia, frases de películas/canciones, diálogos memorizados?
   ☐ Sí, mucho → "Eres un procesador gestalt"
   ☐ A veces
   ☐ No

5. Intereses especiales / vocablos motivadores:
   ___________
```

### Implicación en la app
- **Si gestalt** → rama NLA: modelo gestalts flexibles ("vamos a ___", "quiero más ___"), luego mezcla/variación, luego palabras aisladas.
- **Si analítico** → rama actual: palabra → 2 palabras → marcos → gramática.
- **Si mixto** → combinar ambas ramas en paralelo.

---

## 🔴 FALTANTE CRÍTICA 2: LME Tracking y Progreso Medible

### Problema actual
La app mide "estrellas" (motivacional) pero no mide el verdadero progreso lingüístico (LME, espontaneidad, variedad léxica).

### Cambio requerido
**Panel de seguimiento clínico** (para familia/logopeda):

```
📊 PROGRESO LINGÜÍSTICO
├─ LME actual: 2.8 morfemas (Etapa II de Brown)
│  └─ Objetivo próximo: 3.2 (Etapa III) en 4-6 semanas
├─ Tendencia LME: 📈 +0.3 últimas 2 semanas
├─ % espontáneo: 45% (vs 55% provocado)
├─ Variedad léxica (type-token): 0.68 (bueno, >0.60)
├─ Funciones usadas: [pedir, rechazar, comentar, preguntar:0%]
└─ Desde: 2025-06-01 | Sesiones: 47 | Horas: 12:34

💡 Insight: LME crece pero necesita más PREGUNTAS (función no usada aún)
   → Próximo juego: "¿Qué ves?" / "¿Dónde está?"
```

**Cálculo de LME en la app:**
- Contar morfemas por enunciado (palabra → palabras + desinencias/flexiones).
- Promediar sobre últimos 20-50 enunciados válidos.
- Comparar con tabla de etapas de Brown.

---

## 🟡 FALTANTE MODERADA 1: Semántica por Colores (Colourful Semantics)

### Problema actual
El constructor actual (categorías en pestaña) no visualiza la estructura sintáctica de forma clara y escalable.

### Cambio requerido
**Constructor de frases por bloques de color** (alternativa o complementaria a actual):

```
┌─ QUIÉN (naranja) ─┬─ QUÉ HACE (amarillo) ─┬─ QUÉ (verde) ─┐
│                   │                       │               │
│ [  Yo        ]    │ [   quiero        ]  │ [ galleta ]   │
│ [  Mamá      ]    │ [   come          ]  │ [ agua    ]   │
│ [  El perro  ]    │ [   ve            ]  │ [ coche   ]   │
└───────────────────┴───────────────────────┴───────────────┘
             ↓ Toca bloques para crear frases

[  Yo  ] [  quiero  ] [  galleta  ]
                    ↓
            "Yo quiero galleta"  🔊

┌─ Agregar más? ─┐
│ ☐ DÓNDE (azul)     │
│ ☐ CUÁNDO (morado)  │
│ ☐ CÓMO (rojo)      │
└──────────────────┘
```

**Ventajas:**
- Estructura visual clara (cada color = un constituyente).
- Escalable: empieza con 2 colores (sujeto+verbo) → luego 3 (+ objeto) → luego 4+ (+ lugar, tiempo).
- Compatible con Colourful Semantics (framework reconocido).
- Transitable a CAA: cada bloque puede ser un pictograma.

**Implementación:**
- Modal en nivel 2-3 (no obligatorio en nivel 1, que usa marcos).
- Cada bloque muestra un banco de palabras categorizadas por color.
- Al completar, reproduce con voz + consecuencia.

---

## 🟡 FALTANTE MODERADA 2: Modo Gestalt Real (NLA)

### Problema actual
No hay rama específica para usuarios que procesan por gestalts/ecolalia.

### Cambio requerido
**Rama "Gestalts y Recombinación"** en onboarding (si gestalt detectado):

```
📽️ GESTALTS Y MEZCLA (para procesadores gestalt)

Modelo de gestalts útiles:
├─ "Vamos a ___"      (petición flexible)
├─ "Quiero más de ___" (recurrencia)
├─ "Yo veo ___"       (comentario)
├─ "¿Dónde está ___?" (pregunta memorizada)
└─ Gestalts personalizadas (cargadas de películas favoritas, ecolalia actual del usuario)

Actividades:
1. Modelado: app dice el gestalt con entonación, contexto motivador.
2. Repetición opcional (no obligatoria).
3. Mezcla graduada:
   - "Vamos a [VERBO]"
   - "Vamos a [VERBO + OBJETO]"
   - Variantes de "vamos a" (vamos, vamos aquí, vamos rápido)
4. Extracción: resaltar palabras sueltas dentro del gestalt.
5. Recombinación espontánea: usuario mezcla gestalts ("vamos a quiero parque").

Sin penalizar ecolalia ni repetición: es el proceso.
```

---

## 🟡 FALTANTE MODERADA 3: Expansión/Recast Modelado y Coaching del Adulto

### Problema actual
La app sugiere palabras pero no modela la expansión clara de lo que el usuario produjo.

### Cambio requerido
**Sección "Expansión" y tarjetas de coaching**:

```
Usuario toca: [yo] [quiero] [galleta]
App muestra:  ✅ Muy bien. Aquí ves más completo:
              
              "Yo quiero UNA GALLETA"
              
              O así: "Yo quiero LA GALLETA ROJA"
              
              ¿Te gustaría agregar dónde, cuándo o cómo?
```

**Panel "Consejos para la familia/cuidador":**
```
💡 CÓMO AYUDAR A [nombre] A ALARGAR SUS FRASES

Esta semana el objetivo es: agregar "dónde" a las peticiones.

Ejemplo:
  Él dice: "Quiero agua"
  Tú repites: "Sí, quieres agua... ¿dónde? ¿en la cocina o aquí?"
  (Espera 3-5 segundos)
  
Hoy lo practicamos: [snack, baño, parque]

Tarjeta imprimible: adjunto PDF con imágenes y frases.
```

---

## 🟡 FALTANTE MODERADA 4: Actividades Naturalistas ("Trampas de Comunicación")

### Problema actual
La app es funcional pero poco contextualizada en escenarios motivadores reales.

### Cambio requerido
**Mini-actividades donde alargar la frase es necesario** para lograr el objetivo (refuerzo natural):

```
🎮 ACTIVIDAD: "La Máquina de Snacks"
────────────────────────────────
Visual: máquina expendedora con botones de colores.

Mecánica:
1. Usuario elige un snack (toca imagen).
2. Máquina dice: "¿Quieres una... qué?"
   (espera, señala el objeto, vuelve a esperar)
3. Usuario DEBE DECIR o tocar:
   - Opción 1 palabra: "Galleta" → máquina dice "mmm, hace falta más" 
   - Opción 2 palabras: "Quiero galleta" → máquina avanza
   - Opción 3+ (ideal): "Quiero una galleta salada" → ¡éxito! Suena la recompensa, cae el snack

Refuerzo natural: obtiene lo que pidió.
Andamiaje: niveles de ayuda (marco completo → marco parcial → independiente).
Interés: snacks/objetos personalizados.
```

Ejemplos adicionales:
- "El juego de los cubos" (apila solo si pide con palabra por color y tamaño).
- "¿Adónde vamos?" (elige lugar + cómo llegar).

---

## 🟡 FALTANTE MODERADA 5: Rama Gestalt Mejorada + Panel Diagnostico

### Registro de Gestalts del Usuario
```
📝 GESTALTS OBSERVADOS EN [usuario]

Entrada del cuidador:
"Gonzalo dice: 'Uno, dos, tres, ¡arriba!' cuando quiere jugar a levantarse"

App guarda → detecta patrón "arriba" = levantarse
→ modela variantes ("arriba mamá", "arriba más", "arriba cama")
→ resalta "arriba" para extracción
```

---

## 🟢 MEJORAS INMEDIATAS (bajo esfuerzo, alto impacto)

1. **Onboarding expandido**: preguntas del perfil (analítico vs gestalt, LME inicial, intereses).
2. **Panel de progreso básico**: mostrar LME actual vs. objetivo de etapa.
3. **Instrucciones literales en admin**: cambiar "marcos personalizados" por ejemplos claros.
4. **Tarjeta de coaching semanal**: sugerir una técnica al adulto basada en el progreso actual.
5. **Rastreo de funciones comunicativas**: pedir, comentar, rechazar, preguntar, narrar — mostrar cuáles faltan.

---

## 🟠 MEJORAS MEDIANAS (1-2 meses, arquitectura moderada)

1. **Constructor de frases por colores** (Colourful Semantics) como alternativa.
2. **Rama gestalt** con modelado y mezcla gradual.
3. **Panel de progreso clínico** (LME, type-token, espontaneidad, tendencias).
4. **Expansión modelada** (lo que escriben → versión expandida).
5. **Mini-actividades naturalistas** (máquinas expendedoras, juegos contextualizados).

---

## 🔴 MEJORAS MAYORES (3+ meses, requiere rediseño)

1. **TTS mejorado**: reconocimiento de voz que registre **enunciados reales** (no solo toques) para calcular LME verdadera.
2. **Integración con logopedas**: acceso a informes, objetivos compartidos, feedback en tiempo real.
3. **Narrativa escalada**: secuencias de imágenes para conectar frases (inicio-nudo-desenlace).
4. **CAA tira-de-frase**: para usuarios mínimamente verbales.
5. **Base de datos de gestalts y ecolalia**: biblioteca de patrones comunes en autismo para modelar.

---

## 📋 Checklist de implementación (Roadmap)

### Sprint 1 (esta semana)
- [ ] Expandir onboarding: analítico vs gestalt, LME estimado
- [ ] Agregar campo "intereses motivadores" al perfil
- [ ] Panel básico de progreso: LME actual + objetivo

### Sprint 2 (próximas 2 semanas)
- [ ] Tarjetas de coaching al adulto (técnicas Hanen-style)
- [ ] Rastreo de funciones comunicativas (pedir/comentar/preguntar/rechazar)
- [ ] Expansión modelada: usuario produce → app muestra versión +1 nivel

### Sprint 3 (siguientes 3 semanas)
- [ ] Constructor Colourful Semantics (modo alternativo)
- [ ] Rama gestalt: modelado de gestalts flexibles + mezcla
- [ ] Mini-actividades naturalistas (máquinas, juegos contextualizados)

### Sprint 4+ (futuro)
- [ ] Integración con logopedas
- [ ] TTS mejorado para captura de lenguaje espontáneo
- [ ] CAA tira-de-frase
- [ ] Panel clínico avanzado (type-token, tendencias, recomendaciones)

---

## 📚 Referencias integradas en la app

Documentar en "Acerca de" > "Basado en investigación":
- Etapas de Brown (LME)
- Modelos NLA (Marge Blanc) para procesadores gestalt
- Colourful Semantics (Alison Bryan)
- PECS (Bondy & Frost)
- EMT (Milieu Teaching)
- Hanen (capacitación de familias)
- NDBI (PRT, ESDM, JASPER)

---

## 🎯 Principios de implementación

1. **MVP no es perfecto**: empezar con onboarding + panel básico, iterar.
2. **Validación con usuarios reales**: un logopeda + 3-4 familias piloto antes de escalar.
3. **Respeto a neurodiversidad**: ecolalia/gestalt es funcional, no es "mal".
4. **Privacidad clínica**: datos de menores y SLI protegidos (RGPD, consentimiento informado).
5. **Coordinación con profesional**: nunca sustituye al logopeda, complementa.

---

**Documento preparado**: basado en "Cómo fomentar la creación de frases cada vez más largas en personas autistas" — informe de investigación educativa en autismo y CAA.

**Última revisión**: 2026-06-14
