# Tablero: Funnel semanal de leads

Tablero de escalamiento de inversión para Koltin. Sigue a cada **cohorte semanal de
leads** paso a paso por el embudo para responder dos preguntas antes de subir
presupuesto: dónde se están quedando los leads, y si el lead de esta semana vale lo
mismo que el de hace un mes.

`index.html` es el tablero (estático, sin dependencias). Los datos están embebidos en
el `<script>` al final del archivo — se actualizan corriendo las queries de abajo en
Omni y reemplazando los arreglos `COH`, `MAD` e `INV`.

## Regla de lectura

Una cohorte tarda **~60 días** en terminar de pagar:

| Ventana | % de los pagos de la cohorte ya ocurridos |
|---------|-------------------------------------------|
| D7      | 29% |
| D14     | 51% |
| D30     | 75% |
| ~D60    | 100% |

Medido sobre 9 cohortes maduras (1 jun – 26 jul 2026, 848 pagos).

Por eso las últimas 4 semanas **siempre** se ven mal en la columna de pagos. Para
comparar semana contra semana se usa la **ventana fija D7**; el total a la fecha sólo
es comparable entre cohortes de más de 8 semanas.

## Convenciones

- **Cohorte**: fecha de creación del lead en HubSpot (`real_created_at`), no fecha del evento.
- **Semana**: inicio lunes, en todas las tiles. Algunas queries de Omni agrupan por semana
  de domingo — si no se fija el mismo inicio, los totales no cuadran entre tiles.
- **Conteo por paso**: leads *distintos* que alcanzaron el paso, no cantidad de objetos.
  Contar aplicaciones en vez de leads-con-aplicación infla el paso ~40%.
- **Moneda**: MXN. Inversión = Meta + Google.
- **Leads**: HubSpot cuenta todas las fuentes (~2,200/sem); las tiles de inversión sólo
  cuentan los leads atribuidos a Meta/Google (~1,800/sem). El CPL usa los atribuidos.

## Las 7 tiles en Omni

Modelo: **Data Warehouse** (`5c8aa773-c761-4150-8982-e5873dfa51a1`),
topic `Leads and Applications`.

| # | Tile | Query |
|---|------|-------|
| 1 | Funnel por cohorte semanal | https://koltin.omniapp.co/e/1:DCo-hmSP/1 |
| 2 | Tasa de paso a paso | https://koltin.omniapp.co/e/1:nkpHFO55/1 |
| 3 | Ventanas fijas D7 / D14 / D30 | https://koltin.omniapp.co/e/1:mMgDMBSk/1 |
| 4 | Funnel por canal y semana | https://koltin.omniapp.co/e/1:LEuq1ipg/1 |
| 5 | Gasto unificado semanal | https://koltin.omniapp.co/w/a535c33c?key=27 |
| 6 | Performance por canal y semana | https://koltin.omniapp.co/w/a535c33c?key=10 |
| 7 | Curva de maduración | derivada de la Tile 3 sobre cohortes de 8+ semanas |

Las tiles 5 y 6 ya existen en el dashboard *Marketing Performance & Channel Analytics*
y se reutilizan tal cual. Las tiles 1–4 son nuevas: abrir el link, **Save to dashboard**.

La tile 7 se recalcula una vez al mes, no semanalmente.

## Estado al 16 sep 2026

- **CPL +64%** desde el mínimo del 13 jul ($477 → $783 MXN) con gasto plano en ~$1.3M/sem.
  Los leads atribuidos cayeron de 2,458 a 1,783.
- **Respuesta de WhatsApp de 61% a 51%** en 10 semanas. Es el indicador que se mueve
  primero cuando la calidad del lead se degrada, y se sabe en 48 horas.
- **La conversión a pago aguanta**: D7 lleva 15 semanas entre 0.5% y 1.6% sin tendencia.
  La caída de pagos en las cohortes de agosto y septiembre es maduración, no deterioro.
