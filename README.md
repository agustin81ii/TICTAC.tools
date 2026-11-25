# TicTac.tools — 1.3.3.7

Suite de escritorio para **ARK: Survival Evolved** que reúne en una sola app las
herramientas que normalmente andan sueltas en diez carpetas distintas: HUD,
macros, crosshair, ajustes de INI, guías de dinos y jefes, Sky Changer y cambio de iconos del inventario.

Todo corre en el cliente. No hay servidor modificado ni nada que dependa de un
servidor: lo único que cambia son archivos locales de tu instalación de ARK, y
todo lo que se toca queda respaldado y se puede restaurar con un clic.

## Módulos

| Área | Qué hace |
|---|---|
| HUD y visión | Overlay de HUD, crosshair, scope, gamma, FOV, loading screen, fonts |
| Input | Macros, autoclicker, hotkeys globales, notifier |
| Config | INI builder, INI changer, launch options, file modifier, file converter |
| Datos | Base de datos de dinos, niveles, precios, bosses, cuevas y dificultad |
| Juego | CompactArk, desync, crafting scripts, line list, map mods |
| Criatura | Guía de niveles |
| Visual | Sky Changer, Custom Lab, paintings, oCBPs, pixel |
| Iconos | Cambio de iconos del inventario a estilo Minecraft (190 items) |

## Lo nuevo en 1.3.4

- **Versión de usuario**: se quitó el bot de crianza. Esta build queda con las
  herramientas de escritorio y el cambio de iconos, sin automatización del juego.
- **Iconos nuevos**: telaraña de Minecraft para el proyectil de red, mechero para
  el detonador de C4 y espada de madera para el club.
- El preset ahora tiene 192 ítems.
- Se quitó la dependencia de OCR: el instalador ya no arrastra el motor de texto.

### Íconos

Preset `minecraft` con 192 ítems: armaduras, armas, munición, recursos,
consumibles y accesorios. Todo se aplica y se revierte desde la app, con respaldo
automático de cada textura.

---|---|
| `ItemIcon` | tooltip y silueta del slot |
| `Albedo Map` del `ItemIconMaterialParent` | grilla del inventario |

Además, cada textura reparte su cadena de mips entre el `.uasset` suelto (256/128)
y uno de los contenedores SeekFree de ~2 GB (64 px hacia abajo). El escritor:

1. Construye un **índice de 5.364 texturas** leyendo la tabla de mips de cada
   export en los tres contenedores.
2. Resuelve el offset absoluto de cada nivel; los mips 3+ se encadenan con
   `offset(n+1) = offset(n) + tamaño(n) + 32`.
3. Encodea el sprite a BC3 y escribe **toda la cadena**, en el archivo suelto o
   en el contenedor según la flag del mip.
4. Respalda solo los rangos tocados (no copia 2 GB) y los registra para restaurar.

El índice se cachea y solo se reconstruye si cambian los contenedores (~1 segundo).

## Instalación

1. Descargá `Instalador-TicTac.tools-1.3.3.7.exe` desde **Releases**.
2. Ejecutalo. Instala por usuario en `%LOCALAPPDATA%\Programs\TicTac.tools`,
   sin pedir permisos de administrador.
3. Crea acceso en el escritorio y en el menú Inicio, y trae su propio desinstalador.

Self-contained: no hace falta tener .NET instalado.

## Stack

- .NET 10 · MAUI Blazor Hybrid (WinUI 3, sin empaquetar)
- SkiaSharp (escalado nearest, sin interpolar pixel art)
- BCnEncoder.Net (BC1/BC3)
- Inno Setup para el instalador

## Notas

- Cerrá ARK antes de aplicar cambios: el juego mantiene los `.uasset` abiertos.
- Los backups viven en `%LOCALAPPDATA%\TicTac.tools\IconBackups`.
- ARK: Survival Evolved es marca de Studio Wildcard. Este proyecto no está
  afiliado ni respaldado por ellos.
