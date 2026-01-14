Construir una web app tipo Trello (kanban) mínima pero real, con:

Tablero con 3 columnas (To do / Doing / Done)

Crear/editar/eliminar tarjetas

Drag & drop entre columnas

Persistencia local (LocalStorage) como MVP

UI limpia y usable

Tests básicos y typecheck para feedback loop

Objetivos

Validar que Ralph puede construir features UI iterativamente.

Tener un MVP usable tipo Trello en pocas iteraciones.

Forzar feedback loops: typecheck + tests + lint si existe.

No objetivos

Auth

DB real / multiusuario

Realtime / websockets

Permisos/roles

Usuarios

Usuario único (local) que organiza tareas en columnas.

Requisitos funcionales

Board con 3 columnas por defecto.

Cards con: id, title, description (opcional), createdAt, columnId, order.

Drag & drop para mover tarjetas:

Dentro de la misma columna (reordenar)

Entre columnas (cambiar estado)

Persistencia:

LocalStorage (clave kanban_v1)

Cargar al iniciar; guardar en cada cambio

Accesibilidad mínima:

Botones con labels

Navegación usable sin mouse (no perfecta, pero no rota)

Requisitos no funcionales

UI rápida, sin lag notable con 50–100 tarjetas.

Código limpio, comentado solo donde aporte.

Componentización clara.

Arquitectura recomendada (para que Ralph no se pierda)

state central (Zustand o Context+Reducer; usar lo que ya tenga el repo)

Tipos TS en types.ts

Persistencia en lib/storage.ts

UI components: Board, Column, Card, CardModal o CardEditor

Definición de Done

Todas las historias en prd.json pasan (passes: true)

npm run typecheck pasa

npm test pasa (al menos tests de reducer/store y persistencia)

Drag & drop funciona en navegador

Persistencia funciona (refresh conserva board)

User Stories (right-sized para Ralph)
US-001: Scaffold UI del Board (sin lógica)

Como usuario
Quiero ver un tablero con 3 columnas
Para entender el layout.

Acceptance

Página /kanban (o ruta equivalente) muestra columnas “To do”, “Doing”, “Done”

Cada columna tiene header + contador (0)

Estado inicial hardcoded (sin store)

Responsive básico

US-002: Modelo de datos + state central

Como developer
Quiero un store con columnas y tarjetas
Para manejar estado consistentemente.

Acceptance

Tipos TS: Column, Card, BoardState

Store con acciones:

addCard(columnId, cardData)

updateCard(cardId, updates)

deleteCard(cardId)

moveCard(cardId, toColumnId, toIndex)

Estado inicial: 3 columnas vacías

npm run typecheck pasa

US-003: Crear tarjeta (UI + store)

Como usuario
Quiero crear tarjetas en una columna
Para agregar tareas.

Acceptance

Botón “Add card” por columna

Form inline o modal: title requerido, description opcional

Al guardar, aparece la tarjeta y aumenta contador

Validación: no permite title vacío

US-004: Editar y eliminar tarjeta

Como usuario
Quiero editar/eliminar tarjetas
Para mantener el board actualizado.

Acceptance

Click en tarjeta abre editor (modal o panel)

Edita title/description

Botón delete con confirm simple

UI no se rompe con textos largos

US-005: Drag & drop entre columnas (MVP)

Como usuario
Quiero arrastrar tarjetas entre columnas
Para mover tareas de estado.

Acceptance

Drag card → drop en otra columna funciona

Al soltar, actualiza columnId y orden

Reordenar dentro de misma columna funciona (si la lib lo facilita)

Si hay que elegir librería: usar una estable (ej: dnd-kit) o la que ya exista en el repo

Verify in browser (criterio obligatorio)

US-006: Persistencia en LocalStorage

Como usuario
Quiero que el tablero se guarde automáticamente
Para no perder cambios al refrescar.

Acceptance

Carga initial state desde LocalStorage si existe

Guarda en cada mutación

Botón “Reset board” que limpia storage y vuelve a vacío

Manejo de errores: si storage corrupto, fallback a vacío sin crashear

US-007: Tests mínimos (feedback loop real)

Como developer
Quiero tests automáticos
Para asegurar que Ralph no degrade el estado.

Acceptance

Tests para:

addCard agrega card con id único

moveCard cambia columna y orden

persistencia: serialize/deserialize (o wrapper storage)

npm test pasa

US-008: Pulido UI + UX rápida

Como usuario
Quiero una experiencia agradable
Para usar el MVP sin frustración.

Acceptance

Loading state (instantáneo, pero sin parpadeos)

Empty state por columna

Contadores correctos

Acciones visibles en hover/focus

Mantener estilo minimal/pro

Comandos de calidad (Ralph debe usar)

npm run typecheck (o el equivalente real del repo)

npm test

npm run lint (si existe)

Si frontend: npm run dev para verificación manual/automática
