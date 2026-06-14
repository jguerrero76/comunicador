# Comunicador - Arquitectura & Diseño AAC

**Comunicador** es una aplicación web de comunicación aumentativa y alternativa (AAC) diseñada para adolescentes con autismo, basada en investigación de terapia del lenguaje.

---

## 📋 Índice

1. [Principios de Diseño](#principios-de-diseño)
2. [Arquitectura](#arquitectura)
3. [Categorías de Vocabulario](#categorías-de-vocabulario)
4. [Niveles de Ayuda](#niveles-de-ayuda)
5. [Motor de Sugerencias](#motor-de-sugerencias)
6. [Sistema de Estrellas](#sistema-de-estrellas)
7. [Tecnología](#tecnología)
8. [Fuentes de Investigación](#fuentes-de-investigación)

---

## Principios de Diseño

Comunicador se basa en **5 principios clave de AAC para autismo**:

### 1. **Vocabulario Nuclear** (75-80% de comunicación diaria)
Palabras de alta frecuencia que aparecen en todos los contextos:
- **Verbos**: quiero, tengo, fui, estoy, he ido, me gusta, puedo
- **Conectores**: a, en, con, porque, pero, cuando
- **Emociones**: bien, mal, contento, cansado, nervioso

### 2. **Marcos de Frase (Sentence Frames)**
Estructuras que se repiten para guiar la combinación de palabras:
- "Hoy he ido a ___"
- "Estoy ___ porque ___"
- "Quiero ___ con ___"

### 3. **Apoyos Visuales**
- 🎨 Emojis en cada palabra (feedback visual inmediato)
- 📸 Pictogramas ARASAAC del banco de imágenes API
- 🖼️ Fotos personalizadas de personas conocidas (Candela, Dima, Logos)

### 4. **Scaffolding Progresivo**
3 niveles de ayuda que crecen con la autonomía:
- **Nivel 1**: Solo marcos de frase predefinidos (máxima estructura)
- **Nivel 2**: Marcos + sugerencias inteligentes contextuales
- **Nivel 3**: Completa autonomía, sin sugerencias (solo filtro de verbo)

### 5. **Retroalimentación Inmediata**
- 🔊 Habla cada palabra al tocarla (confirmación auditiva)
- 📊 Contador de palabras + barra de progreso hacia hitos
- ⭐ Estrellas por frases de 3+ palabras (motivación gamificada)
- 💬 Toast notifications para acciones (borrar palabra, etc.)

---

## Arquitectura

### Estructura General

```
index.html (2000+ líneas, archivo único)
├── HTML (línea 280-390)
│   ├── Header + Barra de estrellas (contador/progreso/premio)
│   ├── Pantalla de temas (9 temas con emojis)
│   └── Pantalla de chat (construcción de frases + respuestas)
├── CSS (línea 8-268)
│   ├── Tema: fondo azul oscuro (#0F172A), tarjetas azul claro (#E0F2FE)
│   ├── Colores por categoría (CAT_COLORS)
│   └── Animaciones (confetti, toasts, transiciones)
└── JavaScript (línea 414-2163)
    ├── Datos (TOPICS, WORDS_BY_CAT, NEXT_PATTERNS)
    ├── Motor de sugerencias (getSuggestions, getSuggestionsCore)
    ├── Voz (Web Speech API para TTS + STT)
    ├── Persistencia (localStorage)
    └── Admin panel (niveles, pictogramas personalizados)
```

### Flujo Principal

```
Usuario selecciona tema
    ↓
Aparece starter AAI + palabra-banco por categoría
    ↓
Usuario toca palabras o habla
    ↓
Se construye frase (máximo 3 pestaña de palabras)
    ↓
Se actualizan sugerencias contextuales
    ↓
Envía (validación: mín 3 palabras + verbo)
    ↓
Recibe feedback: hito de estrellas + respuesta de IA
    ↓
Repite (contexto del tema se mantiene)
```

---

## Categorías de Vocabulario

La app agrupa palabras en **14 categorías** (color-coded):

| Categoría | Emoji | Color | # Palabras | Función |
|-----------|-------|-------|-----------|---------|
| **⚡ Verbos** | ⚡ | 🔴 Rojo (#EF4444) | 23 | Núcleo: acciones y estados |
| **🔗 Conectores** | 🔗 | 🟠 Amber (#F59E0B) | 27 | Preposiciones, temporales, lógicos |
| **👤 Quién (Sujetos)** | 👤 | 🔵 Azul (#3B82F6) | 16 | Pronombres, personas cercanas |
| **😊 Emociones** | 😊 | 💗 Rosa (#EC4899) | 19 | Estados emocionales |
| **🧩 Terapias** | 🧩 | 🟣 Púrpura (#8B5CF6) | 15 | Contextos terapéuticos (Dima, Logos...) |
| **😎 Mi día** | 😎 | 🟠 Naranja (#F97316) | 9 | Actividades cotidianas |
| **👾 Juegos** | 👾 | 🟢 Verde (#22C55E) | 13 | Consolas, videojuegos |
| **🍿 Series** | 🍿 | 🔴 Rojo (#EF4444) | 12 | Películas, plataformas |
| **🦁 Animales** | 🦁 | 🌊 Teal (#14B8A6) | 16 | Mascotas, fauna |
| **😋 Comida** | 😋 | 🟠 Dark Orange (#EA580C) | 18 | Alimentos, lugares de comida |
| **🚀 Lugares** | 🚀 | 🔵 Sky (#0EA5E9) | 14 | Destinos, espacios |
| **🎸 Música** | 🎸 | 💜 Purple (#A855F7) | 19 | Artistas, canciones, géneros |
| **📚 El cole** | 📚 | 🟢 Emerald (#10B981) | 13 | Escuela, compañeros, materias |
| **🏆 Deportes** | 🏆 | 🔵 Sky (#0EA5E9) | 14 | Actividades físicas, equipos |

**Total: ~200 palabras nucleares + contextuales**

---

## Niveles de Ayuda

El usuario puede cambiar el **nivel de andamiaje** en la pantalla de admin:

### Nivel 1: **Marcos de Frase** 🏗️
- **Cuándo usar**: Principiante, dificultad severa
- **Muestra**: Solo 12-15 marcos de frase predefinidos (ej: "Hoy he ido a", "Me gusta porque")
- **Sugerencias**: Ninguna (cero libertad)
- **Admin extra**: Permite agregar marcos personalizados

**Marcos por defecto:**
```
• Hoy he ido a ...
• Hoy estoy ...
• Hoy he comido ...
• Me gusta mucho ...
• No me gusta ...
• Me siento ...
• Quiero ir a ...
• Ha sido ...
• Ayer fui a ...
• Esta mañana he ...
• Lo mejor ha sido ...
• Estoy contento porque ...
```

### Nivel 2: **Palabra a Palabra** 🎯
- **Cuándo usar**: Intermedio, capacidad variable
- **Muestra**: Categorías completas + sugerencias contextuales inteligentes
- **Límite**: 14 sugerencias por contexto
- **Lógica**: Analiza últimas 1-3 palabras → propone continuaciones sensatas

**Ejemplos:**
```
"Hoy he ido a" → sugiere: "casa", "cole", "parque", "Dima", "Encajales"
"Con" → sugiere: "mamá", "papá", "mis amigos", "mi familia"
"Porque" → sugiere: "me gusta", "es divertido", "es fácil", "mola mucho"
```

### Nivel 3: **Autonomía Total** 🚀
- **Cuándo usar**: Avanzado, escritura independiente
- **Muestra**: Solo 8 sugerencias (mínimo)
- **Filtro**: Debe haber un verbo (validación sintáctica)
- **Objetivo**: Que escriba libremente con guía ligera

---

## Motor de Sugerencias

### Algoritmo `getSuggestions()`

```javascript
if (nivel === 1) {
  // Mostrar solo marcos de frase predefinidos
  return DEFAULT_STARTERS.slice(0, 12)
}

if (nivel === 3) {
  // Mínimo scaffolding: solo filtro de verbo
  return getSuggestionsCore(text, { noTopicFill: true, limit: 8 })
}

// Nivel 2 (por defecto): scaffolding completo
return getSuggestionsCore(text, { limit: 14 })
```

### Algoritmo `getSuggestionsCore()` (el cerebro)

1. **Autocompletar** (si no hay espacio): palabras que comienzan con lo que escribió
2. **Patrones contextuales** (NEXT_PATTERNS): últimas 1-3 palabras → qué sigue
3. **Llenar contexto de tema**: palabras de categorías activas en el tema actual
4. **Filtrar repetidos**: no sugiere palabras ya usadas en la frase
5. **Ordenar por frecuencia**: palabras más comunes primero

**NEXT_PATTERNS**: diccionario de ~60 patrones
```javascript
{
  '': ['yo', 'hoy', 'ayer', 'quiero', 'estoy', 'me gusta'],
  'yo': ['he', 'quiero', 'estoy', 'fui', 'tengo'],
  'me gusta': ['mucho', 'porque', 'jugar', 'comer', 'ver'],
  'porque': ['me gusta', 'es fácil', 'es divertido', 'mola'],
  'ayer fui': ['a', 'con', 'al parque', 'a casa', 'solo'],
  'por favor': ['quiero', 'ayuda', 'puedo'],
}
```

---

## Sistema de Estrellas

### Hitos (MILESTONES)

| Palabras en frase | Estrellas | Emoji | Mensaje |
|------------------|-----------|-------|---------|
| 3+ | ⭐ 1 | ⭐ | "¡Muy bien! Vas genial." |
| 5+ | ⭐⭐ 3 total | ⭐⭐ | "5 palabras. ¡Eso es hablar!" |
| 8+ | 🌟 6 total | 🌟 | "8 palabras. ¡Qué frase tan larga!" |
| 12+ | 🏆 11 total | 🏆 | "12 palabras. ¡Eres increíble!" |
| 18+ | 🚀 19 total | 🚀 | "18 palabras. ¡Récord histórico!" |

### Progresión hacia 50 estrellas

- **Meta**: 50 estrellas totales acumuladas
- **Premio**: 🏆 "¡Gonzalo, eres un campeón! ¡50 estrellas significan que puedes jugar a la tablet! 🎮"
- **Visual**: Barra de progreso en el header (actualizada en tiempo real)
- **Persistencia**: Se guardan en `localStorage` ('comunicador_stars')

**Cálculo por sesión típica:**
```
Sesión 1: 5 frases de 5 palabras = 5 × 3 = 15 estrellas
Sesión 2: 5 frases de 5 palabras = 5 × 3 = 15 estrellas
Sesión 3: 5 frases de 5 palabras = 5 × 3 = 15 estrellas
Sesión 4: 1 frase de 8 palabras = 6 estrellas
TOTAL = 51 estrellas → 🏆 Premio conseguido
```

---

## Tecnología

### Frontend Stack
- **HTML/CSS/JS puro** (sin frameworks)
- **Web Speech API**: SpeechSynthesis (TTS) + SpeechRecognition (STT)
- **localStorage**: Persistencia de datos (estrellas, frases guardadas, pictogramas personalizados)
- **Canvas API**: Resize/compresión de imágenes de pictogramas

### APIs Externas
- **ARASAAC API** (`https://api.arasaac.org/api/pictograms/es/search/{word}`)
  - Búsqueda de pictogramas en español
  - Imagen: `https://static.arasaac.org/pictograms/{id}/{id}_300.png`
  - Almacenado en caché (`arasaac_v1` en localStorage)

### Características Técnicas
- **Offline-first**: Funciona sin conexión (una vez cargada)
- **Single-page app**: Todo en un archivo HTML
- **Responsive**: Adapta a móvil (480px breakpoint)
- **Accesible**: ARIA labels, colors contrastados, fuente Nunito amigable

---

## Fuentes de Investigación

Esta app implementa recomendaciones de:

1. **[Speechblubs - Dominando frases de 4-5 palabras](https://speechblubs.com/es/blog/dominando-las-oraciones-de-4-a-5-palabras-gua-de-terapia-de-lenguaje-para-familias)**
   - Estructura de frases cortas
   - Vocabulario esencial

2. **[AAC Plus - Core Vocabulary Strategies](https://blog.aac-plus.com/core-vocabulary-strategies-that-work-beyond-the-classroom/)**
   - Vocabulario nuclear: 25-30 palabras
   - Modelado sin expectativa

3. **[AssistiveWare - Teaching Core Words](https://www.assistiveware.com/blog/teaching-core-words-building-blocks-communication-and-curriculum)**
   - Core words = 75-80% de comunicación
   - Integración en rutinas diarias

4. **[Autism Little Learners - Core Board](https://autismlittlelearners.com/core-board/)**
   - Scaffolding visual
   - Emojis + pictogramas

5. **[Centro Proyecta - Estructuración del lenguaje](https://centroproyecta.es/la-estructuracion-del-lenguaje-en-ninos-con-autismo/)**
   - Marcos de frase repetibles
   - Dificultad: vocabulario amplio pero sin combinar palabras

6. **[Fundación Conecta - Escribir con autismo](https://www.fundacionconectea.org/novedades/blog/escribir-cuando-tienes-autismo)**
   - Apoyo visual (pictogramas)
   - Paciencia y modelos visuales

7. **[Child Mind Institute - Fomentar comunicación](https://childmind.org/es/articulo/fomentar-la-comunicacion-en-ninos-con-autismo/)**
   - Estrategias sociales
   - Ambiente de aceptación

---

## Notas de Desarrollo

### localStorage Keys
```javascript
'comunicador_stars'      // Total de estrellas acumuladas
'comunicador_saved'      // Frases guardadas (máx 30)
'comunicador_recent'     // Palabras usadas recientemente (máx 20)
'arasaac_v1'            // Caché de pictogramas descargados
'custom_pictos'         // Pictogramas personalizados (fotos)
'help_config'           // Nivel de ayuda + marcos personalizados
```

### Variables Globales Principales
```javascript
totalStars              // Total acumulado
sessionStars            // Estrellas en sesión actual
sessionResponses        // Número de mensajes en sesión
sessionBestWords        // Frase más larga en sesión
reachedMilestones       // Set de hitos alcanzados en sesión
helpConfig              // { level: 1-3, customStarters: [] }
currentTopicKey         // Tema actual seleccionado
```

### Funciones Clave
- `getSuggestions(text)` — Motor de sugerencias
- `insertWord(word)` — Insertar palabra + hablar + sugerir
- `sendMessage()` — Validar + enviar frase
- `speakText()` — Hablar frase completa
- `checkMilestones(words)` — Verificar y celebrar hitos

---

**Última actualización**: 2026-06-14  
**Desarrollador**: Claude (Antropic)  
**Beneficiario**: Gonzalo, 14 años, usuario AAC con autismo
