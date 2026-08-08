# Panel de Cobranza — Cuentas por Cobrar Multi-País

Dashboard interactivo construido en HTML, CSS y JavaScript puro que centraliza los indicadores clave de cobranza de una operación multi-país, con filtros dinámicos y seguimiento de facturas caso por caso.

**[Ver demo en vivo](https://victoria2809.github.io/dashboard-cobranza/)** haz click aqui y podras ver la demo! 


## El problema
En cobranza multi-país es normal que la información quede repartida: un reporte de pendientes en un lado, un cálculo de atraso hecho a mano en Excel en otro, y el motivo real de por qué una factura puntual no se cobra (una disputa, una promesa de pago) que termina viviendo en la memoria de alguien o perdido en un mail. Si alguien pregunta "¿cómo estamos con la cobranza de Brasil este mes?", hay que cruzar todo eso a mano.

Quise ver si podía resolver eso con un panel simple.

## Qué hace
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

Los KPIs se recalculan en tiempo real según los filtros, no son valores fijos
El selector de agencia se ajusta solo según el país que elegiste, para no mostrar agencias que no correspondan
Los comentarios quedan guardados con localStorage, así el contexto de una factura no se pierde de vista aunque recargue la página
La tabla prioriza automáticamente lo más urgente en vez de mostrar todo en el orden en que se cargó
El gráfico de barras apiladas por país usa Chart.js
Es un solo archivo HTML — nada de npm install ni pasos de compilación, así que se puede abrir y editar directo

## Interfaz

Diseño responsive, con la misma identidad visual del resto del portfolio.

## Stack

| Capa | Tecnología |
|---|---|
| Lógica / estado | JavaScript vanilla |
| Gráficos | Chart.js (vía CDN) |
| Estilos | CSS puro |
| Persistencia de comentarios | localStorage del navegador |
 
