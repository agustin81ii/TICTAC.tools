# Cómo cambiar los iconos a tu gusto

Guía para armar tu propio set de iconos en TicTac.tools 1.3.3.7.

## 1. Dónde vive el preset

Cada preset es una carpeta con un JSON adentro:

```
%LOCALAPPDATA%\Programs\TicTac.tools\wwwroot\presets\icons\minecraft\
        minecraft.json
        iron_pickaxe.png
        ark\                 (sprites del mod arkcraft)
        block_wood.png       ...
```

El nombre del JSON tiene que coincidir con el de la carpeta. En la tarjeta
**CUSTOM ARK → Iconos de inventario** aparecen todos los presets que encuentre ahí.

## 2. El formato del JSON

```json
{
  "name": "minecraft",
  "version": 1,
  "entries": [
    { "ark": "PrimalItem_WeaponMetalPick", "icon": "iron_pickaxe.png", "px": 16 },
    { "ark": "PrimalItemResource_Wood",    "icon": "block_wood.png",  "px": 32 }
  ]
}
```

| Campo | Qué es |
|---|---|
| `ark` | nombre de clase del item en ARK (ver punto 3) |
| `icon` | archivo PNG dentro de la carpeta del preset. Puede estar en subcarpeta: `ark/hide.png` |
| `px` | tamaño con el que se dibujó el sprite. Es informativo: se usa para la vista previa |

## 3. Cómo saber el nombre de clase de un item

Es el `PrimalItem...` del blueprint. Tres formas de sacarlo:

- **Wiki de ARK**: en la página del item, sección *Spawn Command*. En
  `.../PrimalItem_WeaponMetalPick.PrimalItem_WeaponMetalPick` el nombre de clase
  es el último segmento antes de la comilla: `PrimalItem_WeaponMetalPick`.
- **En el juego**: consola con el comando de spawn del item, mismo criterio.
- **Desde la app**: la tarjeta lista todos los items del preset con su nombre; los
  que no resuelven aparecen como *sin ruta*.

## 4. Cómo hacer tu sprite

- PNG cuadrado, **fondo transparente**.
- 16×16 si querés el look Minecraft clásico; 32×32 o 64×64 si querés más detalle.
- Se escala con *nearest*, así que el pixel art no se deforma ni se difumina.
- Dejá un par de píxeles de margen: el juego dibuja el icono casi a sangre en la ranura.

## 5. Ejemplo completo: cambiar el pico de metal

1. Cerrá ARK.
2. Copiá tu PNG en `...\presets\icons\minecraft\mi_pico.png`.
3. Abrí `minecraft.json` y agregá (o editá) la entrada:

```json
{ "ark": "PrimalItem_WeaponMetalPick", "icon": "mi_pico.png", "px": 64 }
```

4. Abrí la app → **CUSTOM ARK → Iconos de inventario**.
5. Tocá **Aplicar**.

Si querés volver atrás, **Restaurar** devuelve todo a original.

## 6. Armar un preset propio

1. Creá la carpeta `...\presets\icons\mine\`.
2. Adentro, `mine.json` con la misma estructura.
3. Meté tus PNG en esa carpeta.
4. En la tarjeta, elegí `mine` en el selector de preset.

No hace falta tocar código ni recompilar.

## 7. Qué parchea la app por vos

Un item de ARK muestra dos texturas distintas:

| Textura | Dónde se ve |
|---|---|
| `ItemIcon` | tooltip y silueta del slot |
| `Albedo Map` del material del icono | grilla del inventario |

La app resuelve las dos sola y escribe **toda la cadena de mips**, en el
`.uasset` suelto o en los contenedores SeekFree, según corresponda. No tenés que
preocuparte por offsets ni formatos.

## 8. Cosas que conviene saber

- **Cerrá ARK antes de aplicar.** El juego mantiene los `.uasset` abiertos y la
  escritura falla con `being used by another process`.
- Los backups quedan en `%LOCALAPPDATA%\TicTac.tools\IconBackups`, y de los
  contenedores solo se respaldan los bloques tocados (no 2 GB).
- El índice de texturas se cachea en `...\ark-icon-index.json` y solo se rehace
  si cambian los contenedores.
- Si un item no aparece en la lista, el nombre de clase está mal escrito o el
  item no existe en tu instalación.

## 9. Si algo no se ve

1. ¿Aplicaste con ARK cerrado? Si el juego estaba abierto, la tarjeta muestra los
   errores y no escribe.
2. ¿Reiniciaste el juego? Las texturas se cargan al abrir.
3. ¿El item tiene material propio? Algunos items dibujan la grilla con una
   textura distinta del tooltip; la app ya parchea las dos, pero si ves una sola
   cambiada, avisá con el nombre del item.
4. **Restaurar** y volver a aplicar descarta cualquier estado mezclado de pruebas
   anteriores.
