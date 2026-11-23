# Forge Seed Finder (Minecraft 1.20.1 / Forge 47.x)

Mod **solo de cliente** (funciona en singleplayer y en servidores sin que el servidor lo tenga)
que recupera la semilla del mundo en dos fases:

1. **Structure seed (48 bits)**: pilares del End (16 bits) + posicion de estructuras / tesoros enterrados.
2. **Semilla completa (64 bits)**: compara los biomas que has visto con los que generaria cada una de las 65536 semillas posibles.

## Compilar
1. Instala JDK 17.
2. Descarga el MDK de Forge 1.20.1 (47.3.0) desde files.minecraftforge.net y copia `gradlew`, `gradlew.bat` y la carpeta `gradle/` a esta carpeta.
3. `./gradlew build` -> el jar queda en `build/libs/forgeseedfinder-1.0.0.jar` (ponlo en `.minecraft/mods`).
   Para probar desde el IDE: `./gradlew runClient`.

## Uso
Comando: `/seedfinder` (autocompleta).

1. **Overworld**: camina. Automaticamente se detectan tesoros enterrados (cofre en el bloque 9,9 del chunk, rodeado de bloques) y se guardan muestras de bioma.
2. **Estructuras**: ponte en el centro de la estructura y ejecuta `/seedfinder structure <tipo>`
   (o con chunk explicito: `/seedfinder structure village 12 -5`).
   Funciona mejor con estructuras de tipo "jigsaw" (village, pillager_outpost, ancient_city, nether_complex):
   en una aldea ponte junto a la campana. En estructuras pequenas (templos, igloo...) el chunk de inicio puede ser el vecino; si el comando dice que el chunk es invalido, prueba otro.
3. **End**: ve al End y mira la isla central hasta ver `Pilares del End detectados: 10/10` (minimo 7).
4. `/seedfinder crack` -> structure seed. Necesita ~30 bits de informacion (3-4 estructuras; un tesoro vale ~6.6 bits, una aldea ~9.4). `/seedfinder status` muestra cuantos llevas.
5. Vuelve al Overworld, camina por varios biomas (>= 30 muestras) y `/seedfinder biomes` -> semilla del mundo.

Otros: `status`, `pillars`, `undo`, `reset`, `crack force`, `biomes <fallosPermitidos>`, `structureseed <valor>`, `treasure [x z]`.

## Limitaciones
- Requiere visitar el End (los pilares dan los 16 bits que hacen viable la busqueda).
- Los datos se pierden al desconectarte (se reinician con el logout).
- La fase de biomas asume Overworld vanilla (no large biomes / amplified / biomas de mods / datapacks).
- `crack` hace 2^32 candidatos por pillar seed: con pocos nucleos puede tardar varios minutos; `biomes` mas aun.
- Las sales/espaciados de estructuras estan escritos de memoria para 1.20.1: si una estructura nunca da resultados, repite con otra.
- Usalo solo donde este permitido: muchos servidores prohiben este tipo de herramientas.

## Obtener el .jar sin instalar nada (GitHub Actions)
1. Crea un repositorio en github.com y sube el CONTENIDO de esta carpeta (no la carpeta en si; `.github` debe quedar en la raiz).
2. Pestana **Actions** -> espera a que termine el workflow "build" (unos 3-5 min).
3. Entra a la ejecucion y descarga el artefacto **forgeseedfinder-jar**; dentro esta el .jar para tu carpeta `mods`.
