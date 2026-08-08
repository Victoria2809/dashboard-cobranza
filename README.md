# Panel de Cobranza — Cuentas por Cobrar Multi-País

Dashboard interactivo construido en HTML, CSS y JavaScript puro que centraliza los indicadores clave de cobranza de una operación multi-país, con filtros dinámicos y seguimiento de facturas caso por caso.

**[Ver demo en vivo]((https://github.com/Victoria2809/dashboard-cobranza/blob/main/final%20dashboard%20github%20testeoo.html)/)**

## El problema

En cobranza multi-país, la información suele vivir repartida: un reporte de facturas pendientes acá, un cálculo de días de atraso hecho a mano en Excel allá, y el contexto de por qué una factura puntual no se cobra (una disputa, una promesa de pago) que solo alguien recuerda de memoria o queda perdido en un hilo de mail. Responder "¿cómo estamos con la cobranza de Brasil este mes?" implica cruzar varias fuentes a mano.

## La solución

Un panel único donde se puede:

- Ver de un vistazo cuántas facturas están pendientes, pagadas o en disputa
- Filtrar por país, agencia o estado para aislar un segmento puntual
- Ver el % de cobranza y el promedio de días que tarda en pagarse una factura
- Dejar un comentario por factura (por ejemplo, por qué una está en disputa) para que ese contexto no se pierda

## Cómo funciona

```
Se generan facturas (dataset de ejemplo)
        ↓
Se aplican los filtros activos (país, agencia, estado, búsqueda)
        ↓
Se recalculan los KPIs y el gráfico sobre el subconjunto filtrado
        ↓
La tabla se ordena por urgencia (disputas primero, luego atrasos)
        ↓
Los comentarios se guardan en el navegador al editarlos
```

## Características técnicas

- **KPIs dinámicos**: facturas pendientes, pagadas, en disputa, % de cobranza (por monto) y promedio de días para cobrar — todos recalculados en tiempo real según los filtros activos.
- **Filtros encadenados**: el selector de agencia se ajusta automáticamente según el país elegido.
- **Comentarios editables por factura**: persisten en `localStorage`, así el contexto de una factura puntual (ej. "en disputa por cargo duplicado") queda documentado.
- **Ordenamiento por urgencia**: la tabla prioriza automáticamente facturas en disputa y pendientes más atrasadas.
- **Gráfico de estado por país**: barras apiladas (pagada / pendiente / en disputa) usando Chart.js.
- **Sin dependencias de build**: es un solo archivo HTML, no requiere `npm install` ni proceso de compilación.

## Interfaz

Diseño responsive, con la misma identidad visual del resto del portfolio.

## Stack

| Capa | Tecnología |
|---|---|
| Lógica / estado | JavaScript vanilla |
| Gráficos | Chart.js (vía CDN) |
| Estilos | CSS puro |
| Persistencia de comentarios | localStorage del navegador |
 
