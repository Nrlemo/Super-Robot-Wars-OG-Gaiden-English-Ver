# SRW OG Gaiden (PS2) — traducción JP→EN · estado del proyecto

_Actualizado: 2026-10-25. El historial de sesiones (lo que antes iba en este encabezado) está en
[`work/notes/HISTORIAL.md`](work/notes/HISTORIAL.md); las sesiones nuevas se anotan ahí y acá queda solo el estado._

### Estado real (2026-10-26) — **RC01** (ex f120)

Fuente de verdad del estado: **issues de GitHub** (115 cerrados, 4 abiertos) y los dumps; las notas fechadas de abajo son historial y pueden estar superadas.

| | |
|---|---|
| **Build candidata** | `work/build/SRWOGG_EN_RC01.iso` (md5 `7e834ed083969bc379b43cc2a57714d8`; era `SRWOGG_EN_f120.iso`). Flags: `build_test.py --texto --cartas --texturas --dialogo-w 24 --dialogo-h 28 --fichas-h 24 --terreno-dx 4 --pool` (= `./build.sh`) |
| **Parche** | `work/build/release/SRWOGG_EN_RC01.xdelta` (2.673.179 B, md5 `ddd82a81…`) sobre la JP `cb7dc448…`; aplicado da la RC01 byte a byte (`cmp` OK). Checksums en `release/CHECKSUMS_RC01.txt` |
| **Vista en pantalla por el mantenedor** | f118/f119/f120 confirmadas (#118 cartel Final Stage, #119 icono de arma, #120 pantalla de agradecimiento). #105–#117 se cerraron el 2026-10-25 tras sus capturas |
| **Sin probar** | **hardware real / OPL con la RC01** (#34 abierto; la última prueba en OPL fue el 2026-10-24) y regresión completa (#35: menús, combate, escenas, backlog, guardado/carga, Spirits, BGM, objetivos) |
| **Issues abiertos** | #84 Unit Stats: rótulos HP/EN duplicados (texto + sprite; propuesta: vaciar el texto, `necesita-iso`) · #76 revisión humana de juegos de palabras/registros de las frases de batalla · #34 D5 plataformas y tester en hardware · #6 épica Fase 5 |

**Texto en el dump** (medido 2026-10-26, filas con `en` / filas): frases de batalla 48.128/48.128 · script de historia 20.613/20.721 · base de datos 19.517/19.849 · ELF + overlays 3.846/8.055 (traducibles: **3.855/3.867, 99,7 %** según `lote_menu.py estado`; el resto son grilla de nombres y filas no traducibles) · misc 2.367/3.379. Total ≈ 94.470/100.130. **Todo es borrador propio: nada revisado línea a línea** salvo lo que el mantenedor vio en pantalla.

**Hecho desde la última actualización del README**: fuente/VWF y texturas de UI en inglés, fichas y menús con letra a H = 24, diálogo W = 24/H = 28, glosario con enlaces `<término>` arreglado (#98/#99), cuelgue del glosario (#92) y de `prologo` (#96) cerrados, Funds/Move Type/Pilot Stats/Weapon Stats/Parts arreglados (#103–#117), cartel Final Stage (#118), icono de arma (#119) y pantalla de agradecimiento con QR (#120, hoja `GRAPHIC:0/152`).

**Próximos pasos reales**: (1) probar la RC01 en OPL/PS2 real y correr la checklist de regresión (#34, #35); (2) cerrar #84; (3) revisión humana de frases (#76) y del resto del texto; (4) publicar el parche (el mantenedor deja material en `work/test/capturas_mantenedor/para_publicar/`); (5) triar la captura nueva `capturas_mantenedor/Kanji_en_library.png` (kanji en la Library) en un issue. **Antes de crear un release**, `build_test.py` sigue emitiendo `jugar_SRWOGG_EN_f<N>.py`; renombrar a mano o parametrizar.

> Desde aquí hasta «Próximos pasos» es historial de sesiones anteriores; las cifras de la tabla siguiente son de 2026-10-19.

### Estado histórico (medido el 2026-10-19)

**Texto en el dump** (`work/dump/*.jsonl`, filas con `en`):

| Bloque | Filas | Con inglés | Estado |
|---|---:|---:|---|
| Script de historia (`MAP/14`) | 20.721 | 20.613 | borrador propio, sin revisar; cortes de línea rehechos a 387 px (`lote.py cortes`, #62; antes 348) |
| ELF + 7 overlays (menús) | 7.832 | 3.632 | 3.632/3.644 traducibles (`lote_menu.py estado`; las 12 restantes son la grilla de ingreso de nombre, no se traducen); borrador, sin revisar |
| Frases de batalla (`BATTLE/5`) | 48.128 | 48.128 | **las 157 tandas traducidas** (21.496/21.496 JP únicos, 2026-10-19; rama `battle-prueba-real`); borrador sin revisar; bloque `battle` de `reinsert.py` probado solo con sintético |
| Base de datos (`IM`, `SHARED`, `SHARED2`, `SBBATTLE`) | 18.202 | 18.202 | **Toda la base de datos con inglés (2026-10-19)**: biblioteca (`DB:`, 8.417 filas) (las 175 sin inglés son cadenas vacías): armas, habilidades, Spirits, nombres, etapas (títulos, BGM y resúmenes de OG1/OG2/OGG), datos de unidad/bonus/partes y los 1.183 textos largos (descripciones de unidad, perfiles, glosario, ayuda); **Shuffle Battler `SBDB`** (2.386, nombres/cartas/BGM/descripciones) y **frases `SBMSG`** (7.399 filas, 5.448 JP únicos, 154 tablas) — las filas vacías del JP quedan vacías. Todo traducido desde el JP con `biblio.py`; sin revisar. **Problema abierto (#67):** `IM:3/0` se deja en japonés (la ID build solo leyó `SHARED:3/0`, 0 de 2.307 lecturas en `IM`; `DB_SHEETS_EN` en `reinsert.py`) y `IM` ya no desborda; `SHARED` (+0x4ee8 B) **se mueve al sobrante vacío de `BATTLE.BIN`** (`build_test.py`, #67/#29; probado en PCSX2: ruta de la Library completa OK; falta OPL/hardware) |
| Misc (ayuda, tutorial, BGM, terrenos, fondos) | 3.379 | 2.367 | **terrenos (453), BGM (278), tutorial (745), ayuda (674) y fondos de combate de Free Battle (217, `BATTLE/13/0`, `fondos.tsv`) traducidos** (2026-10-19, `misc_en.py`, tablas en `work/trans/misc/`, #78): mismo marcado que el JP, ancho ≤ renglón JP más ancho de la pantalla (VWF); todo in situ, `IM`/`SHARED` no crecen; el tutorial se escribe en `IM/10` y en su copia `SHARED/10`. Quedan PADT (nombres internos de efectos, con números de cuadro y basura del editor) y DEBUG (`テストコマンド／NNN` ×256): por contenido no se ven y quedan en JP (#50) |

≈ 76.300 de ≈ 98.000 filas (78 %). Ninguna línea revisada.

**Validaciones** (2026-10-18, logs en `work/logs/`):
- Tests (`cd work/tools && python3 -m unittest tests.test_<x>`): 12 módulos OK, 13 tests salteados (necesitan
  ISO/build). `pytest` desde la raíz no los encuentra (imports relativos a `work/tools`).
- `fit.py`: **0 errores** (antes 19; ver abajo; también después de rehacer los cortes del diálogo, #62), 21.390 validadas, 575 `jp_floor`, 68 `box_below_jp`, 2.853 sin caja.
- Smoke `etapa1` sobre `completa_v2` (`work/logs/smoke/etapa1_20261018_125412/`): llega al final (New Game → diálogo →
  objetivos → mapa → Spirits → fin de turno → combate), 0 alertas de memoria, pila 0x9900 B. Las 11 «fallas» son
  texto distinto de las referencias, grabadas con una ISO vieja con menos inglés (revisado a ojo: nada roto).
  Regrabarlas con la v2 (`smoke.py run … --grabar`) después de la prueba en hardware.
- `reinsert.py check`: sin avisos salvo «sin inglés» (battle, misc, db). MAP achica −0x1500 B (18 hojas crecen; el
  script va al heap del mapa, sin tope en RAM, #41).

**Cuelgue del glosario (#92) — causa identificada** (2026-10-24, PCSX2, `SRWOGG_EN.iso` `ef5393f7…`; sin cambios de datos todavía): el cuadro con el popup de un término enlazado supera el búfer de la lista de dibujo del GS (2 × `0x80000` B en `0x1E40000`/`0x1EC0000`, ~465 B por glifo ≈ 1.127 glifos de tope, el juego no comprueba el final; en PCSX2 `DMA Ch.1 does not terminate`, `D1_TADR=0x1EC0000`). Vista sola = 776 glifos, con popup ~1.130–1.170. En JP no pasa (renglones de ~22 glifos contra ~75). Mismo mecanismo que #80 y probable causa de #96. Herramienta: `work/tools/gs_lista.py` (mide el uso de la lista por PINE). **Arreglo (2026-10-24, `work/patches/vwf.asm`):** los búferes se reservan en `FUN_00156f40` (`0x156F4C/0x156F60/0x156F68`, tamaños y base por `lui`); pasan a `0xA0000` B cada uno (A `0x1E40000`, B `0x1EE0000`, tope ≈1.409 glifos), hasta `0x1F80000` = inicio de la pila. El tramo `0x1F40000–0x1F80000` estaba en cero y sin referencias `lui` en el ELF ni en los overlays. Con una copia de la ISO EN parcheada a mano (no es la build oficial): glosario → EOT 3 de 3 veces sin cuelgue, popup completo, 1.171 glifos = `0x84E20` B. Falta: rehacer `./build.sh` y probar en OPL (y combate/Spirit/Shuffle por si algo usaba ese tramo). **Enlaces de la Library (#98, 2026-10-24):** `reinsert.enc(links=True)` (por defecto en `Ctx.enc`) codifica `<título>` sin alinear `<`/`>` a pares y con el interior igual que el título (última letra a 2 bytes si el total es impar); un corte de renglón dentro del enlace deja un espacio al final del primero. En el dump se corrigieron 89 filas cuyo enlace no coincidía con el título (`Armored Modules` → `<Armored Module>s`, `Personal Troopers`, `Energy Field`, `Shura God`, `Earth Federation Forces`…). Verificador: `work/tools/check_links.py` (+ test); quedan sin título en el JP también `special humanoid mobile weapon` y `T-Link`. En una ISO de prueba (`build_test.py`, no la oficial) «Aidoneus Island» muestra `EOTI Institute`, `EOT` y `DC` como enlaces. Falta probar un enlace partido en dos renglones y abrir cada uno en OPL. **Diálogo doble con la vista de términos (#99, 2026-10-24):** reproducido en PCSX2 (stage 1, línea de Foglia con `<White Star>`, Cuadrado = «Show Terms»). Causa: esa vista redibuja las 4 líneas del diálogo encima con un objeto texto global (`0x41F9C0`, W = 20) desde `0x2A1150` (llamadas a `f5540` en `0x2A11A4`/`0x2A11D0`); con W = 22 en el diálogo salían dos copias (la segunda ~88 % de ancho, = 20/22). Arreglo: `dlg_scale.py` desvía esas 2 llamadas al stub de W (`ELF_SITES`); probado en caliente (una sola copia, texto verde sobre el original) y verificado en la ISO de prueba (`0x2A11A4/0x2A11D0` = `jal 0x14E3E0`). También: 12 enlaces `<…>` del diálogo no coincidían con ningún título (`Super Robots`, `PTs`, `Isurugi`, `Tesla Leicht Institute`…; no se resaltaban) → corregidos; verificador `check_links_map.py` + test. **Marcado de color `#NN…##` (#90, 2026-10-24):** causa en `w_16f620` (`0x16F620`): recorre el texto de a pares y retoma el emparejado al final de cada control (`#NN` = 3 B, `##` = 2 B), no desde el inicio del string. `reinsert.enc(pairs=True)` alineaba los controles contra el inicio, así que tras `#05` el `##` de cierre caía en medio de un par: salía un `#` literal y se comía la letra siguiente (el color quedaba abierto). Arreglo: `enc` lleva la paridad del tramo (`shift`, cambia tras cada `#NN`) y alinea contra eso; test `test_marcado_color_cierre_alineado` (recorre como el juego) y `test_pares` ajustado. Las 45 cadenas ELF con `#NN` siguen entrando en su capacidad. Falta rehacer la build y mirar en OPL Buy Cards, aviso de Personal Data y premio inicial.
  Fila de iniciales de Spirits (#91, 2026-10-24): el pie de la pantalla de Spirits es una sola cadena `%s#%02d%s` (W1OL `0x17a94`, 21 marcas de 3 B desde `0x66dcb0`) que dibuja `w_16f620`. El formato (`0x66de68`, ASCII, fuera de la reinserción) lleva ahora un espacio de 2 B (`0x8140`, 4 px) tras cada marca, parcheado en `build_test.py`; con 1 B el siguiente `#NN` caería a media pareja (#90). Fila estimada ≈ 384 px-base (cabe antes del icono R3). Falta OPL.
  Unit Stats «HP HP» / «EN EN» (#84, 2026-10-24): **no es un defecto**: la ISO JP dibuja igual el rótulo de texto (ＨＰ/ＥＮ) y, pegado a la barra, el sprite italic «HP»/«EN» (captura `work/test/capturas_claude/jp_unit_stats_hp_en.png`, ruta smoke sobre la ISO JP). Cerrado sin cambios; si se prefiere vaciar el texto, es decisión de diseño.

Weapon Stats (#95, 2026-10-24): los rótulos de terreno `W1OL:66b528..40` (Air/Lnd/Sea/Spc) llevan un espacio final (`Lnd ` + rango, 4 B pares como en `2e5cc8`) para que la letra de rango no quede pegada («LndA» → «Lnd A»); «W-Gauge Req.» → «WG Req.» (abreviatura ya registrada en el glosario) en las 5 cadenas, para dejar aire ante el valor «0(170)». Los números comprimidos de Attack/Range/Hit son la fuente numérica del juego (sprite-font, no pasa por la VWF); sin validar en pantalla.

Rótulos pegados (#83, 2026-10-24, parcial): se acortan al ancho del JP (medido con `fit.Font`) los de ancho fijo: `Anim.` (戦闘デモ, 672204), `Rngd.`/`Evade` (Pilot Training/Raise Stats, W1OL 670d58/671458/670200/671470; el «Ranged» de la decisión 2026-10-12 se mantiene en el resto), `Skill Pt` (66e040), `Swaps` (換装武器数), `Pierce` (バリア貫通, W1OL/ELF), `Stg.` (第, 3 W1OL) y `Select Wpn` (武器選択, 5 W1OL). Sin validar en pantalla. Quedan: «Fnds»+valor, nombre de piloto pisado por «Lv», EXP de Rewards y la etiqueta de la pantalla de Spirits.

#83, resto (2026-10-24): «Federation Soldier» → «Fed. Soldier» (73 vs 113 px; el JP 連邦軍兵 mide 72; la caja de nombre del pre-combate llega a ~105 px y el nombre pisaba «Lv») en las 5 cadenas de la DB; «Team» → «Sqd» (`MPOL:4e3f50`, la etiqueta «[Self/Sqd]» baja de 75 a 65 px y deja de tocar el texto del Effect de Spirits). «Fnds» no se toca: en las capturas de Rewards e Intermisión ya deja hueco (30 px < 36 del JP). Sin validar en pantalla.

**Pruebas en emulador de la build completa** (2026-10-19, tanda 1): [`PRUEBAS_EMULADOR.md`](work/notes/PRUEBAS_EMULADOR.md) (#80–#85).
**Kanji sueltos de la UI** (2026-10-19, build `work/build/SRWOGG_kanji1.iso`, md5 `3de55173…`, sin probar en pantalla): los `空陸海宇`, `残/総`, `機`, `第/話`, `個/枚/回/位`, `盾修補`, `援` y `自味敵全隊単` que se veían en japonés **no eran sprite-font**: eran cadenas de 1 carácter que `elf_text.py` descartaba (`is_text` pide ≥ 2). Ahora `kanji_suelto` las acepta si las apunta una tabla de datos (+78) y `tablas_3b` encuentra las tiras de 3 B sin punteros de W1OL: los 5 tipos de movimiento `空陸水宇地` (una letra A/L/W/S/G por casilla) y las 21 marcas de Spirit `熱閃不鉄…` (2 letras). 132 cadenas nuevas traducidas (`menu_kanji_01`); 尸 queda fuera (glifo propio de la fuente). Pendiente verlo en pantalla (anchos de celda, #85/#61/#23).

**Texturas de la UI en inglés** (2026-10-19, trabajo en curso; build `work/build/SRWOGG_tex4.iso`; **vistos en pantalla**: Special Menu, Library, Load («Stage 1»), menú de la intermisión, íconos ATK/CTR de la previa de combate, terreno de Unit Stats (`t3_*`); sin ver: pestañas y carteles de combate; #31/#85): los rótulos de botones, menús, Library, íconos de ataque y carteles de mapa son **texturas**, no texto. `work/tools/tex_en.py` (`preview`/`build`/`lista`) borra el rótulo japonés de la hoja y dibuja el inglés con la misma paleta; 13 grupos declarados (`carteles_mapa`, `iconos_atk`, `etiquetas_combate(2)`, `titulos_intermision`, `menu_rotulos`, `menu_botones`, `special_menu`, `menu_intermision(_cartas)`, `library`); las hojas conservan formato y tamaño. `build_test.py --texturas` las parchea en `WINDOW`/`WINDOW1`/`GRAPHIC`/`MAP`/`MAPSB`/`GRAPHIC2`/`SBBATTLE` (las de contenedores que ya se reconstruyen con el texto entran ahí). Previews en `work/test/capturas_claude/texturas/`. Además `elf_text.py` rescata kanji sueltos sin puntero (copias de tablas que el código indexa) y hay +48 cadenas (`menu_kanji_02`). Pendiente: verlo en pantalla y probar en OPL.
**Arte de las cartas de Shuffle Battler** (2026-10-19, decisión del mantenedor: logos y arte se dejan; build `work/build/SRWOGG_tex5.iso`, sin ver en pantalla): en las 73 cartas con kanji grande de Spirit (`GRAPHIC2:0`, C001–C042, C121–C150, C066, C207, C210) el kanji queda y se agrega el nombre en inglés al pie (grupo `cartas_kanji`, nombres de `shuffle.en.jsonl`). Los rótulos sobreimpresos se traducen: `攻撃力 UP` → `ATK UP` (12 cartas: `cartas_atk`, `cartas_atk_chico`) y `命中判定のみ` → `HIT CHECK ONLY` (4: `cartas_hit`). Para el arte opaco, `tex_en.py` borra por difusión y cuantiza a la paleta de 256 colores de la hoja. Las miniaturas `GRAPHIC2:3` (64×64, el texto mide 2–3 px y no se lee) no se tocan.
**Shuffle Battler y título** (2026-10-19, sin ver en pantalla): carteles del mapa (`MAPSB:1`, 26), rótulos de combate (`SBBATTLE:8`, 9), `GRAPHIC2:5` (LEADER DOWN!!, SUCCESS!/FAILED!, menú VS, TOURNAMENT VICTORY/LOST…), `再開` → CONTINUE (normal y resaltado) y lo que quedaba en los atlas (modos de ataque, MODE del HUD, Support Atk/Ship, ATK/HP REC). `tex_en.py`: `cartel` admite caja, anillos de contorno, tamaño fijo y tiras partidas en dos hojas (`cartel_partido`). Detalle y lo que se deja a propósito en `work/notes/IMAGENES.md` §6. Dudoso: 攻撃力定数 = «ATK BONUS».

**Arreglos tras la tanda 1** (2026-10-19, sin probar en pantalla; build `work/build/SRWOGG_arreglos1.iso`, md5 `aa39cc6b…`): #82 `reinsert.enc` ya no ensancha las letras de `%s`/`%d` (0 de 171 cadenas rotas); #81 textos largos de la biblioteca repartidos a 600 celdas (`biblio.py`, `TOPE_PX`, 1 renglón más que el JP); #80 5 respuestas del Q&A condensadas a ≤ 640 glifos (`misc_en.py` lo exige); #78 introducciones sin `{br}`; #69 `T-Link` unificado (los nombres de Spirit de las cartas coinciden con el menú). Abiertos hasta verlos en pantalla.

**Cortes del guion** (2026-10-20, sin ver en pantalla): con la letra del diálogo a W = 22 (1,1×) la caja con retrato mide 395 px en la escala de `fit`, pero `lote.CORTE_PX` seguía en 348 (el de 1,25×) y partía de más (p. ej. «Hugo: Is it the same one that / tried to absorb the SRX?» cabe en 1 renglón: 376 px). `CORTE_PX` = 387 y escalones (387, 395); `lote.py cortes` rehízo 2.176 de 19.882 mensajes (log `work/logs/cortes.tsv`, respaldo del dump anterior en `work/logs/map_script.jsonl.bak_cortes`). Build de prueba `work/build/SRWOGG_cortes.iso` (md5 `09df2623…`, con las cartas de #93 y estos cortes).

**Textos de las cartas del Shuffle Battler** (2026-10-19/20, #93, sin ver en pantalla): cada carta tiene tres textos (caja de la carta, panel, resumen) en renglones de `Strg` **compartidos entre cartas** (`される。` cierra frases de varias cartas), y el inglés renglón a renglón salía mezclado («…ATK is +15 is +2.»). Ahora cada entrada de `Expl` es un párrafo propio `SBDB:SHARED2/0/0:X<sección>.<entrada>` (1.647: `srwlib/sbcards.py`; tablas `Satk/Ssts/Subs/Cmds` = columnas 6/8/120 (50 en `Cmds`) con índices de `Expl1–4`, `Ecrd/Hcrd` col. 6 con `Expl5/6`; confirmado con las capturas) y [`cartas.py`](work/tools/cartas.py) lo une, limpia (comas, duplicados, mayúsculas), mide con la fuente real y lo reparte en renglones: caja de la carta ≤ 280 (≈ 284 con el punto final que comparte con el panel) y panel ≤ 328 (unidades de `fit.REF_BOX`, calibrados con las capturas: el panel aguantaba 326 y salía del marco con 357; la carta ≈ 290), resumen ≤ 0,8 × el JP más ancho del grupo; tantos renglones como el JP (panel hasta 6); cortes en fin de frase/coma, sin partir nombres ni separar `+<I=NN>`. Lo que no sale bien solo va a mano en [`work/trans/cartas.manual.tsv`](work/trans/cartas.manual.tsv) (`rol⇥JP sin espacios⇥inglés`, 121 párrafos: los contaminados por renglones compartidos y los que no entraban). Cuando el panel repite la carta, reutiliza sus renglones (misma cadena en el pool). `reinsert.build_db` les da renglones propios (los de siempre quedan sin usar y se vacían: solo los que ninguna otra entrada ni columna de tabla nombra) y `fixh.build_pool(compartir=True)` hace que las cadenas iguales y vacías compartan sitio, así que **SHARED2 no crece** (−0x380 B). Comandos: `db_text.py dump` (agrega las filas X) → `cartas.py ajustar` → `reinsert.py check --bloque db`. Build de prueba `work/build/SRWOGG_cartas.iso` (md5 `f7477dd0…`).

**Builds:** la última es `work/build/SRWOGG_completa_v3.iso` (md5 `98b27d34…`; `build_test.py --texto --cartas`;
lanzador `python3 work/build/jugar_SRWOGG_completa_v3.py`): script + ELF/overlays en inglés, tarjetas de capítulo
EN, VWF, con todos los arreglos del 2026-10-18 (`&`, abreviaturas, **prólogos reacomodados**, `Free`). La v2
(`c21ef9c8…`, sin prólogo ni `Free`) pasó el smoke `etapa1`. La v1 (`b31a3437…`) la jugó el mantenedor en PCSX2. En hardware/OPL solo se probó la ISO de
VWF + fuente (2026-10-11), **nunca con texto traducido**. **Prueba de tamaño de letra del diálogo (#62)**: `SRWOGG_dlg_w20/w22/w24.iso` (texto re-cortado + W = 20/22/24 solo en el diálogo; ver [`PRUEBAS_PENDIENTES.md`](work/notes/PRUEBAS_PENDIENTES.md)).
**`./build.sh`** (#32, 2026-10-18): `--null` reproduce la JP byte a byte (md5 `cb7dc448…`, ~1 min); sin argumentos arma `work/build/SRWOGG_EN.iso` (**build completo, 2026-10-19**: `--texto --cartas --texturas --dialogo-w 22`, 27 grupos de texturas con el Shuffle Battler, md5 `ef5393f7…` (2026-10-19, con #81: tope 560 en la biblioteca; antes `ba6dde67…`); lanzador `python3 work/build/jugar_SRWOGG_EN.py`; sin ver en pantalla; la de `--texto --cartas` daba `f323d7e3…` y era reproducible); `--release` genera `work/build/release/SRWOGG_EN.xdelta` (723.556 B) contra la JP, lo aplica sobre una copia y compara md5 (`CHECKSUMS.txt`). Ojo: `SRWOGG_EN.iso` ≠ `completa_v3` (`98b27d34…`), se construyó después; no se probó en pantalla.

**Defectos vistos en `completa_v1`** (capturas en `work/test/capturas_mantenedor/`, 2026-10-18):
- ~~`Strategy Q&A` (W2OL) se veía «Strategy 0\DATA_SP\MAPSB.BIN;1»~~: arreglado (ver abajo).
- Backlog (#60 CERRADO 2026-10-24, visto en pantalla; 1ª versión (41 B) NO entró: el reinsertor aún no reubica al pool y en la build `43f1d12a…` quedó el JP; 2ª versión `:Prev (Fast with` + 13 espacios + `)` = 30 B, ícono R1 fijo a ≈104 px; sin probar): la guía `:Prev (with    :Fast)` no dejaba el ícono R1 en el hueco; abajo de la guía se ve texto de la
  entrada anterior (comparar con el JP).
- Intermisión: `Select Weap…` queda tapado por el cartel `NEW`; un carácter suelto junto a `Skill Points`; título de
  etapa, `第`/`機` y el título gráfico `インターミッション` en japonés.
- Tutorial / Q&A: contenido en japonés (bloque misc/db sin traducir).

**Arreglos del 2026-10-18:**
- `&` pasa a `fit.MIXED_RESERVED`: se escribe en 2 bytes (`＆`, 0x8195). En 1 byte (0x26), `w_16f620` lo toma como
  control (corta en `+ & # /`; el JP lo usa así en 146 cadenas). Afectaba a `W2OL:5cca78`, `W3OL:63c498` y 3 líneas
  del script (`L&E`, `R&D`). Test `test_ampersand_en_dos_bytes`.
- `fit`: de los 19 errores, 17 eran cajas cuyo límite (por vecino) es **más angosto que el propio JP**, que en el
  original no desborda → límite mal medido. Ahora esas cajas dan `box_below_jp` (aviso) y el inglés se valida con
  el piso JP. En cajas que aprietan (`drawer`), si el JP ya iba más apretado que el inglés, no es error (`Twin` en
  `W1OL:673e10`). Solo aplica a `source = "vecino"`; las medidas a mano mandan. Tests `test_box_below_jp`,
  `test_compression_jp_already_worse`. Las 68 cajas `box_below_jp` (`grep box_below_jp work/logs/fit.txt`) son
  lista de trabajo para `box_catalog.py` (#40).
- 2 errores reales: `W2OL:5c30b0` Battle Background → `Battle BG` (caja de 95 px) y `EXOL:478910` AftImg → `AfImg`
  (caja de 42 px).
- **Prólogos (MMOL) reacomodados por ancho** (`srwlib/prologue.py`, capturas del mantenedor: renglones que se salían
  de la pantalla). Cada prólogo es una tabla de 50 ranuras de 0x80 B, un renglón por ranura, blanco = «　» (8140);
  el código cuenta las ranuras con texto y el scroll dura según esa cuenta (verificado en MMOL 0x4e8624–0x4e86a8), así
  que se pueden agregar renglones. Regla del mantenedor (proyecto PSX): dentro de cada párrafo se junta el inglés y
  se corta en los espacios sin pasar **580 px** (= renglón JP más ancho; pantalla de 640, W = 20, calibrado con las
  capturas ±1 %); un renglón que termina en `...`/`…`, `.`, `?` o `!` es corte deliberado. Además se reparten las
  palabras parejo entre los renglones (sin viudas en texto centrado). Prólogo de la historia 27 → 29 renglones; el de
  los Shura 19 → 18. Lo aplica `reinsert.py` (bloque `elf`, tabla entera) y lo valida `fit.py` (caja
  `prologue.scroll`); tests `test_prologue`. La traducción fuente sigue siendo una cadena por renglón JP.
- `Free Space` (pantalla Load, `W1OL:66eec8`, `W2OL:5cbe60`) → `Free`: en la captura se pegaba a la cifra
  («Free Space7555KB»). **Ojo con `box_below_jp`:** ahí el límite por vecino (73 px) era real; la regla lo había
  bajado a aviso. Las 68 `box_below_jp` hay que mirarlas en pantalla, no darlas por buenas.

### Próximos pasos — historial (superado por «Estado real» arriba)

0o. **Prueba de la build completa: #111/#113/#114 y #115–#119 (2026-10-25, falta prueba del mantenedor):** el mantenedor cerró #107–#110 y #112. #114/#111: el espacio 0x8140 de los formatos de cifras salía «░@» (el drawer es de 1 B por carácter) → espacio ASCII (`FORMATOS_ANCHO`). #113: el cursor de Yes/No sí va en x fijas; el relleno de `W1OL:670080` se rehízo con medidas de la captura (18 espacios antes de Yes, «Yes   /    No>»). #106 (comentario del mantenedor): `(%6d)` de Crew/Unit Stats (`W1OL:6706c8`, `670828`) → ` (%6d)`. #89: títulos del `icon.sys` en SJIS fullwidth (como los juegos retail; el ASCII de 1 byte dependía de la BIOS): línea 1 «SRW OG Gaiden» (28 B), «Story #NN», «Personal Data»/«System Data» (al pool); las variantes OGs no llegan a 32 B y su número queda oculto, como antes. #97: 393 decisiones del glosario fusionadas en `decisiones.csv` (las abreviaturas de pantalla quedan como `en_short`); renombres en todo el texto (Kouta, Sänger con glifo nuevo «ä» 0x81AF en `font_build.EXTRAS` + retoque, Lune, Irm, Siegerlion, Grungust Type-0, Neo DC Captain, «Mk.» sin guion, Hückebein con ü, apellido Zonvolt); UI: Quick Save (Q.Save en la ranura de 8 B de `W1OL:66ed98`), Status, ALL, Earned, Barrier Pierce, Evade, Yell (carta C015); «Defend» se mantiene en el comando de combate. #115: «Attribute» del detalle de Weapon Stats → «Attr.» separado por puntero (`elf_split.csv`: `W1OL:65c188`, `65c1a0`). #116: textura `WINDOW1:4/0/9` 特殊能力確認 → «Abilities» (`tex_en.py`, `ops_ruta` de `etiquetas_combate2`). #117: la vista de términos (`0x2A1110`, Cuadrado) también la usa la Library, que se agrandaba a 24×28 por el parche de #99 → `dlg_scale.STUB_TERMS` (`0x14E480`) deja W = H = 20 si en `0x422020` está «EXOL» (la cabecera del overlay); `terreno_dx.STUB_AREA` acotada a `0x14E480`; tests de unicorn (y arreglado `test_only_expected_bytes_change`, que fallaba desde #99). `fit.py`: 40 errores / 9 overflow (los 5 nuevos son de bytes de «Quick Save», resueltos con el pool). #89 confirmado en BIOS por el mantenedor. #118: el cartel «Final Stage» se cortaba: el juego muestra la hoja de `GRAPHIC/8` solo hasta x ≈ 199 (la JP llega a 189) y `card_art.py` medía hasta 236 → `NUM_MAX_X = 196`, primero cierra el tracking y después achica la letra (también entran Stage 10–42, que terminaban en x = 217). Capturas nuevas → #119 (icono «誘» de la lista de armas) y #120 (aviso de salud del arranque, imagen en JP). #119: el icono «誘» (arma guiada: Jammer solo sirve contra ellas) no es texto: es un sprite (grupo `0x13/0x45` de W2OL, un bit de la máscara de atributos que arma `EXOL:0x451190`) del atlas de etiquetas de combate, junto al botón «合体» → «G» y «COMBO» en `WINDOW1:0/0/5`, `2/0/9`, `4/0/9` (`tex_en.ICONOS_ARMA`, `Hoja.icono(oscuro=True)`); decisión del mantenedor «G» → los 5 avisos de Free Battle (`EXOL:479730…479920`) dicen «guided» en vez de «homing» (`decisiones.csv`: 誘導兵器 = Guided weapon). `fit.py`: 39 errores / 9 overflow. Build `work/build/SRWOGG_EN_f119.iso` md5 `28957d54…` (mismas opciones que `build.sh`; f117 + #118 + #119).
0p. **Pantalla de agradecimiento (#120, 2026-10-25, build `SRWOGG_EN_RC01.iso (ex f120)` md5 `7e834ed0…`, falta prueba del mantenedor):** el aviso de salud del arranque NO es texto ni video: es la hoja **`GRAPHIC:0/152`** (RAW8 704×512, CLUT 0x400 + índices; `GRAPHIC.BIN` sector 26908) que el juego sube a VRAM en dos mitades de 704×256; se halló con un volcado GS del mantenedor (`.gs.zst`: la VRAM va dentro y los `IMAGE` de los paquetes traen el píxel exacto; buscado el fragmento en `GRAPHIC.BIN`). Se reemplaza en sitio (mismo tamaño) con `tex_en.py` (grupo `agradecimiento`, op `pantalla`, método `Hoja.reemplazar`) y `work/tools/agradecimiento.py` (`preview`, `qr`): fondo `work/assets/agradecimiento_fondo.jpg` (estrellas) + dedicatoria en inglés + «Contribute to this project:» + QR de GitHub (`…/Super-Robot-Wars-OG-Gaiden-English-Ver`) y de Buy Me a Coffee (`srwa_en`), módulos de 4 px; ambos decodifican con opencv tras cuantizar y a ×0,8. Requiere `segno` (QR) y `opencv-python-headless` (`qr`). Falta: ver en pantalla (QR legibles, margen vs. el recorte del juego, duración del aviso).
0n. **Build completa (2026-10-25):** `./build.sh` → `work/build/SRWOGG_EN.iso` md5 `760ca6df…`, con todo lo anterior (dumps re-sincronizados con `lote.py`/`lote_menu.py`/`biblio.py`/`frases.py restaurar` y `misc_en.py aplicar`: 0 cambios; `vwf_patch.py build` idéntico). Es igual a f108. Se borraron las ISOs de prueba viejas (f104–f108, probe, w24h28) y sus lanzadores; queda `SRWOGG_ids_v4.iso` (ID build). Falta la prueba del mantenedor (ver `PRUEBAS_PENDIENTES.md`).
0m. **Capturas de f105: #108–#114 (2026-10-25, falta prueba del mantenedor):** #108: 13 descripciones de partes (`descripción de parte`, 26 filas) cortadas a ≤ 315 px a W = 20 y ≤ 3 renglones como el JP (el recuadro mide ~284 px a W ≈ 18, medido en captura). #109: `W1OL:66f518` Weapon → Wpn. #110: materiales `S1739/S1740` → Sea/Space Rating Up, `S1746` → Hit Up. #111: la x del costo del Spirit (panel de Pilot Training) = x del nombre + ancho medido de la plantilla `W1OL:673e38`, que nunca se dibuja: «----» → 11 guiones (73 px, más que «Resupply»). #112: `W1OL:6739c0` (primera casilla del Move Type en las listas de unidades) → «A». #113: `W1OL:670080` rellenado para que Yes/No caigan bajo el cursor (x fijas medidas en capturas). #114: `FORMATOS_ANCHO` de `build_test.py` agrega un espacio de 2 B antes del «(» en `670620`/`670680`/`670690` (ahora verifica el relleno de ceros, no un tope de 8 B). `fit.py`: 36 errores / 10 overflow (antes 37/11). Build `work/build/SRWOGG_EN_f108.iso` md5 `760ca6df…`.
0l. **Terreno y Ace Bonus en las fichas (#106, 2026-10-25, falta prueba del mantenedor):** `work/tools/terreno_dx.py` (`build_test.py --terreno-dx 4`, en `build.sh`) corre 4 px a la derecha la letra del rango de terreno: Weapon Stats cambia el inmediato 24.0 → 28.0 (4 sitios en W1OL `0x47620`). En Pilot Stats (`FUN_0016d210`, 4 `jal`) y en el panel de Pilot Training (`FUN_001b65c0`, nodos 58..61, 4 `jal`) la posición sale del layout: esos `jal` van a dos stubs del ELF (`0x14E410`, `0x14E448`; cuerpo muerto de `e3d0`, `dlg_scale.STUB_AREA` acotada a `0x14E410`) que llaman a la original y suman dx a la x. Test con unicorn: `tests/test_terreno_dx.py`. Ace Bonus: las filas `bonus de as (corto)` (174, las que dibuja la caja `w1ol.4e260`) cortadas a ≤ 258 px y ≤ 3 renglones (20 textos / 38 filas). La caja se nombró a mano en `renderers.toml` con el mismo id; solo se quitó ese bloque de `boxes.gen.toml` porque regenerarlo entero mete las grabaciones h104/medir. Panel de Pilot Training: `W1OL:673d08/673d20` → `Rng.`/`Evade` (#105). Falta Develop S.Ammo (otro código). Build `work/build/SRWOGG_EN_f106.iso` md5 `aff3ecfb…`. Capturas de f105 triadas: #108–#114 nuevos, #84 reabierto.
0k. **Etiquetas de Pilot Stats separadas (#105, 2026-10-25, falta prueba del mantenedor):** «Evasion», «Defense» y «Ranged» pisaban el valor en Pilot Stats. Las cadenas (`W1OL:670300/308/310`) las leen 4 tablas de punteros; la de Pilot Stats (`0x657410…`, entradas `0x65741C/20/24`) solo la leen 6 `lw` de las funciones `0x4DD90` y `0x4E320` (barrido de W1OL). Nuevo en `reinsert.py`: `work/trans/elf_split.csv` (id, puntero, en) reapunta **una** palabra de tabla a una copia propia en el pool (`_elf_splits`, exige `--pool`; si la cadena entera ya fue al pool, ese puntero sale de su plan; relecturas igual que el pool). Pilot Stats queda con `Evade` (41 px de 48) · `Rng.` (30 de 51) · `Def` (22 de 50); el resto de las pantallas sigue con la forma larga (Unit Stats 60–61 px, `4f264` 83, `93b08` 64). Riesgo anotado: `4f5c4` (47 px, tabla de tipos `0x6580C0` compartida por 5 sitios, no se puede separar por puntero). Pool: 35 cadenas (32 ya iban antes). Build `work/build/SRWOGG_EN_f105.iso` md5 `d5e08ba9…` (mismas opciones que `build.sh`).
0j. **Cifras y fichas más grandes (#104 cerrado, 2026-10-25, OK del mantenedor):** dígitos de 10 texels (eran 8; `digit_stretch = 1.25` en `retouch.toml`; tope sin pasar la huella del JP en las 335 muestras numéricas de `box_trace`) y alto H = 24 en Pilot/Unit/Weapon Stats (`ficha_scale.py`, `build_test.py --fichas-h 24`, en `build.sh`: 6 estilos copiados con H = 24 e y = −2 en `0x14E7E0–0x14E870`, final libre del área de `e700`; 288 pares `lui`/`addiu` de a1 repuntados en 18 funciones de W1OL). Formatos de #103 reajustados (`%14d`, `%10d`). Build `work/build/SRWOGG_EN_f104.iso` md5 `63875473…`; smoke pilot_status/intermision/status/status_rapido sin cuelgues (la «falla» es `free_watch` sin armar por los datos nuevos en su región, igual que con los stubs del diálogo). Medido: mayúsculas 16 → 19 px de captura, cifras ~+30 % de ancho. Sin tocar: Status del mapa (→ #107) y listas. Pisados de Pilot Stats: #105, #106. Láminas antes/después: `work/logs/font_compare/f104/`.
0i. **Diálogo a W = 24, H = 28 (#102, 2026-10-25):** una sola letra para las dos cajas. Medido en capturas (1,2875 px de pantalla por px de juego): caja del mapa (verde) 438 px útiles, caja de intermisión 569 px (texto desde x=57 hasta 626). Barrido de W sobre 3.000 mensajes: con W=22 hay 4 renglones de hasta 437 px (> 435); W=24 → 0 renglones > 435, máx. 423; W=25 desborda por redondeo; W=26 deja 1 px de margen. `build.sh` y `build_test.py` pasan a `--dialogo-w 24 --dialogo-h 28` (`dlg_scale.py` ahora admite alto distinto; stubs de 5 palabras); `lote.DIALOGO_ESCALA` = 1,2 (`CORTE_PX` = 355) y `lote.py cortes` rehízo 1.892 mensajes: 19.882 mensajes, 0 renglones > 435 px a W = 24 (máx. 425), 0 con más de 3 renglones; 1/2/3 renglones = 10.696 / 7.835 / 1.351. Build `work/build/SRWOGG_EN_w24h28.iso` md5 `7b89fd25…`; visto: diálogos del mapa (etapa 1) y la escena de intermisión «Abandoned Factory» (caja ancha).
0h. **#103 cerrado (2026-10-25):** además de Funds, las 9 cadenas «<efecto> Down/Drain L　（Ｒ/Ｐ系）» del ELF (2e7230…2e7390) llevan 11 espacios finales porque el dígito del nivel se dibuja aparte, ≈40 px antes del final del texto (sondeos: «ABCDEFGHIJKLMN», «-------------L»); «Accuracy Down L» pisaba la «D»; ahora «Acc. Down L 3». Visto en pantalla solo «Acc. Down L» (las otras 8 comparten mecanismo, sin ver). Move Type queda como `Air-W-` (legible; primera celda en palabra). Build `SRWOGG_EN_f103.iso` md5 `0ac3cf6f…`.
0g. **#103 (parcial, 2026-10-25):** columna Funds alineada con `build_test.py` `FORMATOS_ANCHO` (formatos ASCII `%9d`→`%18d` en la lista de Load, W1OL 66eef8; `%8d`→`%12d` en la barra de la intermisión, W1OL 66ec48; cada espacio de relleno mide una cifra) y `Funds` vuelve en la intermisión (66e038). Visto en pantalla en la build `work/build/SRWOGG_EN_f103.iso`. Pendiente de #103: «Accuracy Down L» (D pisada) y Move Type `Air-W-`. #31 cerrado por el mantenedor.
0f. **#100 cerrado (2026-10-25):** quedan des-abreviadas solo las etiquetas con caja medida (`box_trace`/piso JP) o vistas en pantalla; las 12 filas «asumido sin medir» que ninguna captura mostraba (Support, Special×3, Terrain×2, Commander×2, Mobility×3, Final Stage) volvieron a su abreviatura (`Supp.`, `Spec.`, `Terr.`, `Cmdr.`, `Mob.`, `Final`), porque 5 de las 15 asumidas pisaban el valor vecino. Las abreviaturas que siguen se des-abrevian caso a caso al medir la caja (#40/#70). Build `SRWOGG_EN_s100_menu.iso` md5 `b9fdd8fd…`.
0e. **Rutas `spirit_status.toml` (Focus activo → Status: «Spirits in Effect» muestra la marca `Fo`, #85) y `prologo.toml` (New Game, narración del prólogo ~35 s hasta el mapa sin cuelgue, #96)** (2026-10-25, `SRWOGG_EN_s100_menu.iso`, PCSX2 software). Cerrados #85 y #96 (el de #96 sin hardware: probar en OPL).
0d. **Rutas nuevas de smoke (2026-10-25):** `work/test/smoke/status.toml` / `status_rapido.toml` (Status del mapa) e `intermision.toml` (card OGG_100: Load → intermisión → Weapon Upgrade → Unit List/Status/Weapon Stats). Con ellas se vieron y revirtieron 5 des-abreviaciones de #100 que pisaban el valor: `Repair Cost`→`Repair` (4 filas), `Support Attack`→`Sup.Atk`, `Skill Points`→`Sk.Pts`, `Funds`→`Fnds` (intermisión, 2 filas); y los títulos de pestaña `Weapon Upgrade` (66f738, 670e60) → `Wpn. Upgrade` (#63). Build `SRWOGG_EN_s100_menu.iso` md5 `972c59fa…`. Cerrados con captura: #61, #63, #86. Abiertos nuevos: columna Funds sin alinear a la derecha (Load/Intermisión) y glifo pisado en «Accuracy Down L».
0c. **Smoke 2026-10-25 con `SRWOGG_EN_s100_menu.iso` (md5 `6af90010…`, PCSX2 software):** rutas `library` (card OGG_100) y `etapa1` completas sin cuelgues ni alertas de memoria; las «fallas» son solo diferencias contra referencias viejas (texto nuevo), no se regrabaron. Visto: menú de la Library centrado, Scenario Chart resumen etapa 1 («Stage   1», #86), mapa/combate. Corregido a partir de la captura 15_menu_mapa: «Skill Points» tocaba el valor → vuelve a «Sk.Pts» (ELF 2e5f48 y W1OL 66f328). Capturas en `work/logs/smoke/library_20261025_082216/` y `etapa1_20261025_083520/`.
0b. **Menú de la Library alineado (2026-10-25, a confirmar):** `tex_en.py` centra cada rótulo (hoja `library` y `Movie Gallery`/`Gallery` de `menu_botones`) en su caja del JP; antes iban a la izquierda y cada ítem arrancaba en una x distinta (el juego centra cada sprite por el ancho de su caja). Build `work/build/SRWOGG_EN_s100_menu.iso` (md5 `8a5c0242…`, igual que `s100` + esto; `python3 work/build/jugar_SRWOGG_EN_s100_menu.py`). Mirar Library: los 8 ítems deben compartir eje central; si alguno queda corrido, ajustar su caja en `tex_en.py`. Supuesto no medido: que el menú centra los sprites.
0. **Sesión autónoma 2026-10-25 (a confirmar por el mantenedor):** build `work/build/SRWOGG_EN_s100.iso` (`build_test.py --texto --cartas --texturas --dialogo-w 22 --pool`, md5 `20c5fd55…`). Mirar: Move Type en Weapon Upgrade / Unit Stats (antes «Air陸--», ahora letras A/L/W/G), Status del mapa (`Type` y Spirits en efecto: antes kanji), título `Wpn. Upgrade` (#63), comando `Start` de la previa de combate y `Mob.` (W1OL 64db00; en las builds anteriores salían en JP: 戦闘開始/運動性) y las 15 etiquetas des-abreviadas sin medir (#100; lista en HISTORIAL 2026-10-25). Si alguna pisa a su vecina, volverla a la abreviatura en `work/trans/elf_text.en.jsonl` (notas `DESABREV (#100, asumido sin medir)`).
1. **Probar en PS2 real vía OPL** la build `work/build/SRWOGG_EN_86.iso` (md5 `c6f1d39a…`, 2026-10-24; `build_test.py --texto --cartas --texturas --dialogo-w 22`; incluye #60 y #86; la oficial `SRWOGG_EN.iso` `43f1d12a…` no los lleva): ver #60 (guía del Backlog `:Prev (Fast with` + ícono R1 + `)`), #86 (`Stage N` del Scenario Chart, 1 y 2 dígitos) y #96 (cuelgue del prólogo; posible mismo origen que #92). Checklist en [`PRUEBAS_PENDIENTES.md`](work/notes/PRUEBAS_PENDIENTES.md); decisión de plataformas/tester: #34.
   - Cerrados el 2026-10-24 tras probar en OPL: #92 (búferes de la lista de dibujo a 640 KiB), #99 (diálogo doble con «Show Terms»), #98 (enlaces `<término>`), #90 (marcado de color `#NN…##`) y #71 (`box_assign`: las fichas de la Library que dibuja el popup de términos ya no heredan la caja del retrato).
2. **Defectos de pantalla abiertos:** #63 (`Weapon Upgrade` pegado a la pestaña), #81 (textos largos de la Library/perfiles; hace falta captura de la build actual), japonés residual de texturas y sprite-font (#85, #61, #57, #31, #23; p. ej. `Type 陸` en Status). Lección (#60): el reinsertor **no reubica al pool** (solo avisa «reubicable → pool»); una cadena que no entra en su capacidad queda en JP.
3. **Revisar el texto** (frases de batalla #76, biblioteca #68; todo está traducido, sin revisar) y **el desborde de `SHARED`** está resuelto en la build (`SHARED.BIN` se reubica; ver §3.1 «Mover archivos»); falta probarlo en OPL/hardware. La base de datos quedó completa en inglés (2026-10-19): revisar en pantalla los avisos `jp_floor`/`box_dudosa` (SHARED/3/0:E0917… tiene caja de diálogo asignada por `box_assign` pese a ser texto de la Library).
4. **Cajas por rutas dirigidas** (plan aprobado 2026-10-18): `rutas_plan.py` agrupa por pantalla las 637 cadenas
   con `jp_floor`/`box_below_jp` → [`RUTAS.md`](work/notes/RUTAS.md) (intermisión 256, mapa/eventos 249, ELF común
   86, cartas 35, Library 11). Rutas `work/test/smoke/{intermision,library,cartas,mapa_evento}.toml`, cada una
   corrida con `smoke.py run … --box-trace TAG` sobre la ID build y como referencia sobre la build EN (regresión
   #33/#35); card 100 % (`work/emu/memcards/OGG_100.ps2`) solo para Library/cartas y para medir la base de datos
   antes de traducirla. Corte: < ~100 cadenas o < 10 códigos nuevos por pasada; el resto se abrevia al ancho JP.
   **Pasada `ids12` (2026-10-18)**:
   - **`library.toml` hecha y reproducible**: 22 pasos (robots, personajes, glosario, Q&A, diagrama de escenarios), corrida OK sobre la ID build.
     **Faltan sus referencias**: `smoke.py run work/test/smoke/library.toml work/build/SRWOGG_completa_v3.iso --memcard work/emu/memcards/OGG_100.ps2 --grabar`, a cargo del mantenedor.
   - **Intermisión y mapa a mano** (card GameFAQs): solo 10 de las 175 cadenas nunca vistas. Lo que queda no es menú de intermisión sino:
     - mapa: pronóstico de combate, apoyo, condiciones;
     - unidades dobles (`ユニット能力`: los ~55 pares `Ｗ格`/`格射`…);
     - desarrollo de munición especial;
     - W2OL.
     Necesitan un savestate de mapa con un enemigo al lado y una unidad doble, y una ruta por estado.
   - **Fusión**:
     - `box_assign` pasa de 2.228 a 2.435 cadenas;
     - la base de datos pasa de 3.296 a 4.083 filas con caja;
     - `fit` pasa de 562 a 559 `jp_floor`, y `box_below_jp` sube de 68 a 71 (los 3 nuevos hay que mirarlos), con 0 errores.
   - Visto de paso:
     - Q&A de la Library: contenido 100 % en JP y sin extraer;
     - Special Menu y Library: botones de textura en JP;
     - `第`/`話` de Load y del diagrama: **sí son cadenas** de 1 carácter (ver el párrafo de kanji sueltos abajo; #61).
5. **Pool de texto (#30, 2026-10-24; activo en `build.sh` desde la aceptación en OPL):** lo que no entra en su capacidad puede ir a un pool residente en `0x6B0800` (la hoja `GRAPHIC/10/0` se mueve a `0x6B0000` y crece; 4 inmediatos del ELF; sin código nuevo) con todas sus referencias reescritas y releídas; detalle, evidencia y riesgos en [`REINSERCION_ELF.md`](work/notes/REINSERCION_ELF.md) §7. Probado en PCSX2 con `work/build/SRWOGG_pool_test2.iso` (md5 `52f8e3a0…`; 11 etiquetas de W1OL alargadas a propósito, **no es una build para jugar**): ruta `etapa1` completa sin cuelgues, cadenas reubicadas visibles (puntero y `lui`/lo). Hoy ninguna cadena lo necesita (0 avisos «no entra»); sirve para des-abreviar (~228 etiquetas `ABREV` por capacidad). **Prueba del mantenedor en emulador (2026-10-24) con `SRWOGG_pool_full.iso` (`--texto --cartas --texturas --dialogo-w 22 --pool`, md5 `bc076cb8…`): tarjeta del stage en inglés, una batalla y cartas jugadas sin problemas.** **Prueba del mantenedor en hardware (OPL, 2026-10-24): `SRWOGG_pool_full.iso` funciona perfecto** (arranca, menús, mapa, Library, cartas); el mecanismo queda aceptado. `--pool` activado en `build.sh` (con el inglés actual no reubica nada: mismos bytes que sin él, md5 verificado). Des-abreviado: `work/tools/abrev_plan.py` mide la forma completa del glosario contra el piso JP (`work/logs/abrev_plan.tsv`) y aplicó 15 etiquetas SEGURO (Mobility, Barrier, Unit List, Start Battle; excepciones a mano en `work/trans/abrev_full.csv`: ATK, Spirits); entraron en la capacidad `segura`, el pool no se usó. Las que pasan el piso JP solo si se mide la caja siguen en #100 (`abrev_caja.py` las resuelve con grabaciones de `box_trace`). Capturas de OPL: `HP+050MAX+100` literal en la ficha de unidad (corregido: el `+` era marcado de `w_16f620`, ahora `{2b}`), etiqueta de las cartas `Search`/`Potential` cortada (corregido: tope `y=199`), guion suelto junto a «Library» (corregido); «(test)» y «Effect test» son las cadenas alargadas a propósito.
6. **Revisión de texto (#68, #76; 2026-10-24):** pasada hecha y documentada en [`REVISION_76_68.md`](work/notes/REVISION_76_68.md): `Tesla Leich` → `Tesla Leicht` en todo el proyecto (+ decisión en `decisiones.csv`), `Low Morale` → `Low Will` (Shuffle), frase de Char unificada y una línea del script partida (`14/24:B0069`, antes desbordaba 140 px). Quedan decisiones del mantenedor (gritos de fuegos artificiales, `5/244:0156`) y la revisión de estilo de los 1.183 textos largos.
   - **Glosario (#97):** `glossary_lint.py` en 0 errores (2026-10-24; `glossary.csv` regenerado con `glossary_build.py`). Falta que el mantenedor decida las 393 filas de `work/glossary/proposed_review.csv` (columnas `decision`/`en_decidido`; 202 `ui`, 71 `unit`, 23 `character`, 23 `spirit`…); lo aprobado pasa a `decisiones.csv`. Nada de `ref/` (proposed) se fusiona sin esa decisión.
7. **Issues:** sincronizados el 2026-10-24 (cerrados #71 #90 #92 #98 #99).

**Prioridad del proyecto:** la técnica va primero (reinserción, auditoría de overflow/garble, y sobre todo
**rehacer las fuentes**); la traducción/revisión de texto va al final.

Reglas de trabajo: [`CLAUDE.md`](CLAUDE.md). Plan original: fases 0–5 de `CLAUDE.md` §4.
Repo: [`Nrlemo/srw-og-gaiden-en`](https://github.com/Nrlemo/srw-og-gaiden-en) (privado). Pendientes,
errores y decisiones: **issues de GitHub** (índice en §6).

## 1. Resumen

| | |
|---|---|
| **Juego** | *Super Robot Taisen OG Gaiden* (JP) · SLPS-25836 · DVD5 · NTSC |
| **Fase** | Fase 0 cerrada · Fase 1 (extracción) completa salvo texturas (4.167 imágenes ya salen a PNG con `img_extract.py`; faltan `GRAPHIC/3` y `GRAPHIC2/10,11`, ver `IMAGENES.md`) · Fase 2 (VWF, fuente, `fit`) hecha en emulador, falta hardware · Fase 3: todo el texto con borrador en inglés (≈ 94 % de las filas), sin revisar · Fase 4: reinserción integrada (`reinsert.py`), gráficos parciales · Fase 5: `build.sh` + xdelta hechos (#32), falta smoke en hardware |
| **Estrategia** | Pipeline propio sobre la ISO JP, traducido desde el japonés |
| **Problemas a evitar** | render/garble (sprite-font UI, glifos fullwidth, `#` suelto, backlog) y cortes/desbordes (labels, cajas de ancho fijo JP, abreviaturas forzadas) |
| **Máquinas** | local (desktop) por ahora; después migración al servidor |

## 2. ISO base

| ISO | Ruta | md5 | Uso |
|---|---|---|---|
| Dump JP retail (base) | `iso/Super Robot Taisen OG Gaiden (Japan, Korea)/…iso` | `cb7dc4485f46f220c46095c966a3aad0` ✓ | fuente; base del parche |

Extraídos con `iso_ls.py --extract`: `work/extract/jp/` (no versionado).

## 3. Hallazgos técnicos confirmados

### 3.1 Layout del DVD (`work/notes/iso_jp_ls.tsv`)
- ISO9660 sin Joliet, 2.278.464 sectores (4.666.294.272 B). Área de sistema (LBA 0–15) rellena con
  `0x65` — **normal en este dump**.
- Orden físico: `SYSTEM.CNF` (314) → ELF `SLPS_258.36` (315, 2,2 MB) → `OL/*` (1391–3541) → `IRX/*` →
  `DATA_SP/*` → `DATA/GRAPHIC, SHARED, IM, WINDOW, MAP, SQ2, BATTLE, STRM, PSS` → `DUMMY` (1 MiB) →
  10.243 sectores libres al final (~20 MiB). Hasta el tope de DVD5 (4.700.372.992 B) quedan ~34 MB más.
- **Mover archivos (D4, #29; 2026-10-19).** El ELF abre los 10 archivos de datos por nombre al arrancar (`FUN_00168d40`:
  `MAP, IM, BATTLE, SHARED` de `DATA/` y `WINDOW1, GRAPHIC, SHARED2, GRAPHIC2, MAPSB, SBBATTLE` de `DATA_SP/`) y guarda su posición
  como `LBA << 11` en **32 bits**: un archivo así tiene que quedar con LBA < 2^21 (4 GiB). `PSS.BIN` (LBA 2.033.192) ya cruza ese
  límite y los 10.243 sectores libres están después, **no sirven**: `SHARED.BIN` movido ahí cuelga el arranque (pantalla negra,
  probado con la ISO JP y solo ese cambio). Sí sirve moverlo al **sobrante final de `BATTLE.BIN`** (27 MB de ceros fuera del árbol
  LICE; `MAP.BIN` tiene 9,6 MB): `build_test.py` lo escribe en los últimos sectores de la extensión de `BATTLE.BIN` (`HOST_MOVER`)
  y corrige el registro ISO9660 y la File Entry UDF (CRC propio); las dos extensiones se solapan (BATTLE no usa su cola). Si
  hace falta más, el mismo mecanismo vale para otros `.BIN` de los 10 con el sobrante de `MAP.BIN`.
- `SYSTEM.CNF`: `BOOT2 = cdrom0:\SLPS_258.36;1`, `VER = 1.02`, `VMODE = NTSC`.

### 3.2 Formatos y mapa de contenido (detalle: [`work/notes/CONTENEDORES.md`](work/notes/CONTENEDORES.md))
- **LICE/Wpbb** (`DATA/*`, `DATA_SP/*` salvo SQ2/STRM/PSS): formato confirmado, anidado hasta 3 niveles, rebuild
  byte-idéntico. Los archivos raíz tienen sobrante libre al final (MAP ~9,6 MiB, BATTLE ~26 MiB).
- **MWo3** (`OL/*`): cabecera 0x40 + text + data; **MPOL, MMOL y W1OL llenan su slot** (0 B libres: el archivo se carga con la cabecera en `va`, corregido el 2026-10-11).
- **Texto SJIS sin compresión**. Ubicación confirmada: script de historia `MAP/14/*` (`LOGO`, 43 hojas,
  ~20.400 cadenas), terrenos `MAP/12/0`, frases de batalla `BATTLE/5/*` (341 hojas, ~47.000 cadenas), base de
  datos `IM/3/0` y `SHARED/3/0` (`Vers`/`Fixh`), ayuda `IM/8/*` (registros fijos, marcado `<C=7>`), BGM
  `IM/1/0`, tutorial con posiciones `<X=><Y=>` en `IM/10/*` (= `SHARED/10/*`, #49), Shuffle Battler `SBBATTLE/9` + `SHARED2/0`.
  `IM.BIN` = datos de la **intermisión** (sus menús están en W1OL). **Inventario completo con cifras y estado:
  [`work/notes/INVENTARIO_TEXTO.md`](work/notes/INVENTARIO_TEXTO.md)**; dudosos (¿se ven?): `BATTLE/13`, `MAP/15`, `SHARED2/1` (#50).
- **Texturas `P2IG`** (GS nativo: PSMT8/PSMT4 + CLUT). Gráficos con texto: `GRAPHIC/8`, `GRAPHIC2/0`.
- **Fuente SJIS 18×18 4bpp** = `SHARED/0/0` (variante `IM/0/0`), 4480 glifos × 0xB0 B, en RAM en `0xA7B440`;
  índice `(hi−0x81)·192 + lo−0x40` (+ tabla de kanji nivel 2 en `0x2B1DF0` → `idx 0x380+`); drawers
  `FUN_0014fbe0`/`0014f6f0`/`0014f230`/`0014ed50` con ancho proporcional de 3 categorías. Detalle:
  [`work/notes/FUENTES.md`](work/notes/FUENTES.md). Fuente latina propia: 100 glifos (ASCII + `‘“…—`, espacio de
  cifra `0x81AD` y **`ü` en `0x81AE`**, celda vacía de la JP, #47: Hückebein/Wildwürger; va en 2 bytes también en el
  texto mixto y no entra en strings de 1 byte).
- `SQ2` = secuencias de sonido, `STRM` = `ILD`, `PSS` = MPEG-PS → sin texto.
- **Script de historia** (`MAP/14/0..42`, hojas `LOGO`, #16): 10 secciones `(cantidad, offset)`; 0–5 = script
  (no guarda offsets de texto), tabla A (claves internas y **frases de derribo** por piloto) y tabla B (diálogo/rótulos, última = centinela) de offsets u32 a
  un pool SJIS contiguo. El script referencia las cadenas **por índice** → el pool se reescribe libremente.
  20.721 cadenas (17.171 de diálogo). `@` = salto de línea; el nombre `Hablante「` se dibuja como texto (el retrato
  lo pone el script) → en inglés `Name: text`; `<…>` = marcado (el juego oculta los `<>`); `#16`…`##` = color
  (16 = celeste, nombre de ítem; `##` vuelve al normal; `#13` sin ver); `?` = número que pone el script.
  Rótulos sin hablante probados: objetivos (作戦目的, alineado a la izquierda) y avisos de ítem (caja centrada) se
  miden bien con la VWF. En RAM la hoja va en un
  heap (savestates 2–5: `14/34` en `0xFF2880`, seguida de otro archivo): no puede crecer en vivo; en disco sí
  (`lice.rebuild` con `MAP.BIN` del mismo tamaño). Detalle: `work/tools/srwlib/logo.py`.
- **Frases de batalla** (`BATTLE/5`, #17): formato confirmado y extractor (`srwlib/phrases.py`, detalle en
  `CONTENEDORES.md`): S/T/V paralelas por entrada; T = u16 offset al pool, V = voz; lo demás no guarda offsets →
  reescribir el pool solo reapunta T. Round-trip idéntico en las 341 hojas. Una hoja que pase la ranura de `0xA800`
  (ver abajo) deja frases pisadas. Falta decodificar piloto por hoja, situaciones y especiales (#48).
- **RAM para texto que crece (#41)**: la hoja del script va al **heap del mapa** (first-fit `FUN_001574f0`/`FUN_00157360`,
  `0xE80000`+`0x7C0000`): en todos los savestates medidos quedan **≥ 5,2 MB libres** → el script en inglés entra
  (crece ≤ ~120 KB por hoja). Las frases de batalla (`BATTLE/5`) **tienen espacio fijo**: `FUN_001d92e0` le da a
  cada hoja cargada una ranura de **`0xA800` B** (puntero en obj+0x68, arranca en `0x1746800` y suma `0xA800`;
  8 ranuras hasta `0x179A800`, donde empieza otra zona). El juego lee la hoja entera sin tope: si pasa de `0xA800`,
  la hoja siguiente le pisa la cola (probado con `--pad BATTLE:5/*=0x10000`: en combate se vio normal, pero la
  primera hoja quedó cortada justo en `0xA800`). JP: máx. `0x9704` (margen ≥ 4,3 KB); un inglés en
  SJIS de 2 bytes (ELF sin cambios ahí) haría pasar 8 hojas de `0xA800` → frases con la cola rota. Para el inglés
  se decidió **codificar el inglés en 1 byte (ASCII)** en `fbe0` (#42). Relevamiento en RENDERERS §9:
  las frases se dibujan enteras (sin máquina de escribir), el formato no exige alineación a 2 bytes y su único
  código es `/`; límites 255 B por frase y 63 B por línea (W3OL). Espacio: 225 palabras libres verificadas en
  las medidas reescritas. Diálogo del mapa: el splitter de `@` está en MMOL y avanza 2 bytes por defecto
  (parche en el lugar); sin máquina de escribir.

## 4. Herramientas (`work/tools/`)

| Script | Para qué |
|---|---|
| `isolib.py` | Lector ISO9660 mínimo sin dependencias (`Iso.walk()`, `read_file`, `read_at`) |
| `iso_ls.py` | Lista archivos con LBA/tamaño (`--gaps` huecos, `--extract DIR` extrae) |
| `diff_iso.py` | Diff de dos ISOs por archivo → rangos cambiados (`--json`) |
| `setup_env.sh` | Instala el entorno en `work/tools/ext/` (idempotente; local o servidor) |
| `env.sh` | `source`: rutas `SRW_*` desde `config.local.toml` + funciones `pcsx2`, `ghidra`, `ghidra_headless`, `armips`, `imhex` |
| `pcsx2_cfg.py` | Config aislada de PCSX2 (BIOS, PINE 28011, audio muteado, logs) |
| `tex_en.py` | Rótulos de la UI en inglés sobre texturas (`preview`/`build`/`lista`; `build_test.py --texturas`; #31/#85) |
| `info_index.py` | Índice de `info/` → `work/glossary/INFO_INDEX.md` |
| `srwlib/lice.py` | Contenedor LICE/Wpbb: `ls`, `extract`, `verify` (rebuild `--null`), `rebuild --replace RUTA=ARCHIVO` |
| `srwlib/mwo3.py` | Overlays `OL/*.BIN`: cabecera, VA↔offset, espacio libre en el slot |
| `srwlib/sjis.py` | Codec SJIS reversible con etiquetas `{…}` y dialectos de códigos de control; `to_fullwidth` |
| `srwlib/ptrscan.py` | Barrido de punteros u32 a inicios de string; huérfanas/ambiguas; verificación post-reinserción |
| `srwlib/dump.py` | Esquema `Entry` del dump JSONL (`renderer`, `box_id`, `max_px`, `max_bytes`, `ptr_refs`…) |
| `srwlib/gsmem.py` | (Des)swizzle de VRAM del GS: PSMCT32 / PSMT8 / PSMT4 |
| `font_tool.py` | Fuente 18×18: `export`/`import` PNG indexado, `verify` (round-trip) |
| `pine.py` | Cliente PINE de PCSX2: status, lectura de RAM del EE (32 MiB ≈ 3 s), savestates, `write32`; timeout de 10 s por operación (`TimeoutError` en vez de colgarse si hay otro cliente o PCSX2 está colgado) |
| `font_build.py`, `font_compare.py`, `font_ingame.py`, `vwf_live.py` | Fuente latina VWF desde TTF, comparación de candidatas, prueba en vivo |
| `font_retouch.py`, `font_zoom.py` | Fuente propia: semilla TTF + retoque declarativo (`work/font/retouch.toml`); ampliación de glifos |
| `vwf_patch.py`, `vwf_menu_test.py` | Parche VWF del ELF (`work/patches/vwf.asm`) y de MMOL (`work/patches/mmol.asm`, splitter del diálogo en 1 byte, #42): ensamblar / diff / inyectar en RAM; prueba en menús con inglés de relleno |
| `drawer_trace.py` | Traza en ejecución de drawers/envoltorios de texto (hook solo en RAM): sitios de llamada por pantalla, overlay de origen, ablación `--nop`; registro de argumentos (`install_args`) |
| `box_catalog.py` | Grabaciones de `box_trace --watch` → `work/notes/boxes.gen.toml` (ubica sitios sin RAM, límite por borde medido, layout[4] o vecino confirmado) |
| `box_assign.py` | (#40) ID build + grabaciones `ids2`/`ids3` → `work/notes/box_assign.tsv`: caja de cada cadena del ELF/overlays (la más estricta si se vio en varias; avisa si el JP original no entra); `elf_text.py dump` completa `renderer`/`box_id` |
| `lote_menu.py` | Lotes de traducción de los menús (ELF + 7 overlays, `elf_text.jsonl`): `sacar --todos` → 28 lotes `menu_<mód>_NN.jsonl` (orden de memoria, vecinas, bytes in situ, caja en px, sugerencias); `validar` (glifos, `%s/%d`, marcado de `w_16f620`, bytes con `reinsert.enc`, px con `fit`, términos); `integrar` → `work/trans/elf_text.en.jsonl` (versionado). Prompt: `work/trans/PROMPT_MENU.md` |
| `box_edges.py` | Capturas de `box_trace --watch --shots` → borde de ventana candidato por caja + láminas de revisión; `accept N[=X]` → `work/notes/box_edges.toml` |
| `build.sh` | (#32) envoltorio de `build_test.py`: `--null` (ISO idéntica a la JP), por defecto ISO EN, `--release` (+ xdelta aplicado y verificado por md5). Lee `work/config.local.toml` | **Prueba de tamaño de letra del diálogo (#62)**: `SRWOGG_dlg_w20/w22/w24.iso` (texto re-cortado + W = 20/22/24 solo en el diálogo; ver [`PRUEBAS_PENDIENTES.md`](work/notes/PRUEBAS_PENDIENTES.md)).
| `build_test.py` | ISO de prueba para hardware/OPL: copia de la JP + ELF con VWF + fuente nueva (reemplazo del mismo tamaño, verificado); `--logo T.toml`: líneas del script + `MAP.BIN` reconstruido; deja `work/build/jugar_<iso>.py` (abre la ISO en el PCSX2 propio, `emu.pcsx2`); `--pad CONT:HOJA=N` infla una hoja con patrón `PAD!` (solo pruebas de memoria); `--phrases-1b`: todas las frases de batalla con inglés de prueba de 1 byte en su lugar + MMOL parcheado (#42); `--phrases-regla`: todas las frases = regla de 63 caracteres para medir la caja (#40); `--texto` / `--texto-prueba`: reinserta el `en` de los dumps (o inglés sintético) con `reinsert.py` y reconstruye cada contenedor en la copia (`rebuild_container`, #30); el ELF y los overlays del bloque `elf` van en su lugar; `--bloque` elige bloques; `--null`: reescribe el ELF y la fuente originales por el mismo camino y exige md5 == JP (#32); `--texto-ids`: ID build del ELF y los overlays (cada cadena = su código, resto en JP) + `<iso>.ids.tsv` (#30 paso 3); el lanzador `jugar_<iso>.py --memcard` abre con las partidas de GameFAQs en el slot 1 (`memcard.py`) |
| `mem_map.py` | Mapa de memoria libre (#21, [`MEMORIA.md`](work/notes/MEMORIA.md)): `dead` (código muerto del ELF desde `FuncRefs.java` + barrido de `jal`/palabras/`lui` en ELF y overlays → `work/notes/mem_dead.tsv`), `ram` (páginas cero/constantes/variables entre savestates), `check A B` |
| `heap_report.py` | Uso de los heaps first-fit del juego en un savestate `.p2s`, una imagen de RAM o `--live` (#41) |
| `toc_live.py` | Prueba de memoria (#41): reemplaza en RAM el índice cacheado de `BATTLE.BIN` por el de la ISO `--pad` |
| `logo_text.py` + `srwlib/logo.py` | Script de historia (`MAP/14`): `verify` (round-trip de las 43 hojas), `dump` → `work/dump/map_script.jsonl`, `stats` |
| `phrase_text.py` + `srwlib/phrases.py` | Frases de batalla (`BATTLE/5`, #17): `verify` (round-trip de las 341 hojas), `dump` → `work/dump/battle_phrases.jsonl` (48.128 cadenas, dialecto `battle`: `/` → `{br}`), `stats` (tamaño de cada hoja contra la ranura de `0xA800`) |
| `biblio.py` | Biblioteca y Shuffle Battler (`db_text`; grupos `armas, habilidades, spirits, nombres, etapas, datos, textos, shuffle, sbfrases`): `rellenar <grupo>` (`.en.jsonl` = propuesta + `<grupo>.manual.tsv` `id⇥en⇥notas` + `<grupo>.flat.tsv` `JP sin {br}⇥EN plano⇥notas`, que reparte el inglés en los renglones del JP; `\|\|` = párrafo nuevo, ver `wrap_jp`), `tabla <grupo> <archivo>` (`N\|inglés` por línea + macros `@X=…`/`{{X}}`; fuentes en `work/trans/biblio/fuente/`), `lista <grupo> A B` (JP sin traducir), `relleno` (`？？？`, `－－－`, `無し`). Reglas: `shuffle_rules.py` (dados/ATK del Shuffle Battler) y `glossary_as_seiyu.py` (bonus de as). Original: `sacar` (cada ctx + JP una vez, con propuesta de `decisiones.csv` > `glossary.csv` > `ref/armas.csv`) → `work/trans/biblio/{armas,habilidades,spirits}.jsonl`; `validar` (cada copia contra su caja; piso JP = aviso; sufijo `斈孺` conservado), `integrar` (solo `en` en el dump; versiona `work/trans/db_text.en.jsonl` con las notas y `work/glossary/ref/biblioteca.csv`, que el glosario unificado lee primero entre los `ref/` → términos de las tandas de batalla), `restaurar` |
| `frases.py` | Tandas de traducción de las frases de batalla: `tandas` (21.496 JP únicos → 157 tandas ≤ 200 filas en `work/trans/tandas_batalla/`, índice `INDICE.tsv`), `validar`/`integrar` la salida `bt_NNN.en.jsonl` de la IA (copia el inglés a todas las cadenas con el mismo JP; versiona `work/trans/battle_phrases.en.jsonl`), `restaurar`, `estado` (presupuesto de `0xA800` por hoja) |
| `shot_loop.py` | Captura la ventana de PCSX2 cada 1,5 s mientras alguien juega (descarta repetidas; termina al cerrar PCSX2) → `work/logs/<dir>/NNNN.png` |
| `logo_live.py` | Líneas de prueba (`work/test/logo_*.toml`, inglés propio): `check` con fit.py, `inject` en la hoja cargada en RAM (PINE; JP o la de una ISO de prueba con `base =`); códigos `#NN`/`##` tal cual y `{num}` → `?`; `mixed = true`: texto mixto de 1 byte (#42) |
| `cartas.py` + `srwlib/sbcards.py` | Textos de las cartas del Shuffle Battler (#93): `ajustar` (arma el inglés de cada párrafo `SBDB:…:X<sec>.<entrada>`, lo reparte en renglones que entran en su caja —carta 280, panel 328, resumen 0,8 × JP— con cortes en fin de frase y lo escribe en el dump y en `db_text.en.jsonl`; informe `work/logs/cartas.tsv`; `work/trans/cartas.manual.tsv` manda), `ver <sec>.<entrada>` (JP, inglés y ancho de cada renglón). `sbcards`: qué entradas de `Expl` usa cada tabla de cartas y `renglones_de_cartas` (ids de `Strg` que quedan sin usar). Tests: `tests/test_cartas.py` |
| `lote.py` | Traducción/revisión por tandas (Fase 3; flujo y prompt en `work/trans/PROMPT.md`): `plan` (tandas de un nivel o medio, en orden de historia → `work/trans/tandas.tsv`), `tandas` (avance), `sacar --tanda T` (lote `t_T.jsonl` + `t_T.brief.md`: resumen previo, reparto con voz, términos con choques), `resumen T "…"` (→ `work/trans/contexto.tsv`), `sacar 14/N` (lote con hablante EN, tipo de línea, `tm`, `.terminos.tsv`, `.reparto.tsv`), `validar` (ASCII, códigos `#NN`/`{num}`/`<>`, `{br}` final, píxeles y bytes reales, hablante, términos del glosario), `integrar` (→ dump + `work/trans/map_script.en.jsonl`, lo único versionado; rehace los cortes del diálogo salvo `--sin-cortes`), `cortes` (une cada mensaje y lo vuelve a cortar a `CORTE_PX` = 387 px (435 / 1,1 de `--dialogo-w 22`, con 2 % de margen; con 348 —el valor de 1,25×— se partían de más los mensajes que cabían en 1–2 renglones): menos renglones, parejos, corte en fin de frase, sin partir nombres; log `work/logs/cortes.tsv`, #62), `restaurar`, `estado`, `toml` (dump → `work/trans/logo_14_NN.toml` para `build_test`/`logo_live`) |
| `trad_ref.py` | Referencias de traducción desde el JP de OGG: `habla_jp` de cada hablante en `speakers.csv` (pronombre, finales, trato, % formal), `work/glossary/generado/glosario_unificado.tsv` (hablantes + glossary.csv + `ref/` personajes/unidades/armas/términos/escenarios + borrador) y `reporte.txt` (mismo JP con distinto EN: 38 choques) |
| `glossary_gap.py` | Cobertura del glosario contra lo que el juego muestra (biblioteca `db_text.jsonl`, ELF, hablantes) → `work/glossary/generado/faltantes.tsv` (JP sin EN por categoría y frecuencia) |
| `glossary_pilots.py` | Fichas de piloto de Akurasu (solo inglés: OGs y enemigos; 105) → `work/glossary/ref/pilotos_db.csv` (personalidad, bono de as, unidad, crecimiento, relaciones) |
| `glossary_as_seiyu.py` | Bonus de as (119, traducción por reglas con el vocabulario de Akurasu `Ace_Bonuses`) y seiyū (94, Hepburn; 15 con lectura a verificar) → `work/glossary/ref/ace_bonus.csv`, `seiyu.csv` (`proposed`) |
| `glossary_weapons.py` | Armas de la biblioteca (497 nombres): empareja con el glosario ignorando formato (`斈孺`, `{fc4b}`, `・`) y suma propuestas de `work/glossary/ref/armas_manual.txt` (`jp\|en\|?`, editable; `?` = dudosa) → `generado/armas_alineadas.tsv` y `ref/armas.csv` (cobertura 23 % → 100 %; 130 dudosas) |
| `glossary_terms.py` | Nombres del juego sin inglés: términos (49), BGM (136), capítulos (134), unidades (60), personajes (194) a partir de `work/glossary/ref/<x>_manual.txt` (`jp\|en\|?`; `en` vacío = NFKC del JP) → `ref/{terminos,bgm,escenarios,unidades,personajes}.csv` (`proposed`; 88 dudosas). Con `armas`/`ace_bonus`/`seiyu`, cobertura de nombres 100 %; `glossary_gap.py` cuenta además `ui_elf` contra `elf_text.en.jsonl` (52 %) |
| `glossary_review.py` | Cola de revisión (`proposed`/`pending`/conflictos; 441 filas) → `work/glossary/proposed_review.csv` con columnas `decision`/`en_decidido` para el mantenedor |
| `glossary_build.py`, `glossary_lint.py` | Glosario (`work/glossary/`, ver `GLOSARIO.md`): Akurasu + hablantes del script + `decisiones.csv` → `glossary.csv`, `speakers.csv`; validación |
| `box_trace.py` | Cajas de texto en ejecución: layout (W, alto, ancho máx., flag, color), posición en pantalla y ancho estimado por sitio de llamada → `work/logs/box_trace/`. `--watch`: graba en segundo plano mientras se juega (reinstala hooks tras un savestate, avisa si el juego pisa los stubs) |
| `fit.py` + `srwlib/fit.py` | Valida cada string del dump contra su caja **en píxeles** (catálogo `work/notes/renderers.toml`, anchos de la lámina, reglas del parche VWF); sale con 1 si algo no entra. Sin límite medido: piso JP (aviso `jp_floor`, #59) → `work/logs/jp_floor.tsv`; límite por vecino más angosto que el JP: aviso `box_below_jp` y piso JP; sin `box_id`, caja por defecto del renderer (diálogo). Sin ISO; corre en CI (`.github/workflows/fit.yml`) |
| `rutas_plan.py` | Lista de trabajo de cajas por pantalla (#40): lee `work/logs/fit.txt` (`jp_floor`, `box_below_jp`), agrupa por función de Ghidra (ELF, `work/logs/funcrefs.tsv`) o tramo de 0x400 B (overlays) y asigna ruta/estado de partida → `work/notes/RUTAS.md` + `rutas.tsv`. Sin ISO |
| `srwlib/prologue.py` | Prólogos de MMOL (tablas de 50 renglones de 0x80 B): reacomodo del inglés por ancho (580 px, cortes deliberados en `...`/`.`/`?`/`!`, reparto parejo); lo usan `reinsert.py` (escribe la tabla entera) y `fit.py` (valida sin ISO, blancos deducidos del dump) |
| `xinput.py`, `waitscreen.py` | Mando por XTest (usa el mapeo vigente de `[Pad1]` del `PCSX2.ini`) y capturas de la ventana de PCSX2 (X11) |
| `smoke.py` | (#33) Smoke tests en PCSX2 sin pantalla (Xvfb propio, config `work/emu/pcsx2_smoke`, PINE 28012; `SRW_DISPLAY` y `SRW_RENDERER` cambian el X y el renderer, ver §Entorno): `run RUTA.toml ISO` recorre una ruta (`work/test/smoke/*.toml`) y por paso controla vivo (proceso + `free_watch --sin-trampas`), no negra, cambio y parecido con la referencia (pantalla entera ≤ `umbral`; `zona` = recorte de la caja de texto, ruido 0, detecta cualquier cambio de texto); `--grabar` graba referencias (`work/test/smoke/ref/`, no versionadas: capturas del juego); `--sin-watch`; informe + `hoja.png` en `work/logs/smoke/`; `up`/`press`/`shot`/`down` para armar rutas a mano |
| `font_probe.py`, `probe_shot.py`, `probe_burst.py` | Marcar la fuente en RAM para ver qué texto sale de ella |
| `ghidra_run.sh` + `ghidra_scripts/` | Headless sobre el ELF analizado: `DecompileAt` (`!0x…` crea función), `DisasmRange`, `ListFunctions`, `FindTextParser`, `Xrefs` |
| `lice_index.py` | Índice de hojas de cada contenedor → `work/notes/containers/*.tsv` (magic, texto, entropía) |
| `misc_en.py` | (#78) inglés del bloque misc desde tablas jp → en (`work/trans/misc/terrenos.tsv`, `bgm.tsv`, `ayuda.tsv`, `tutorial.tsv`, `fondos.tsv`): `aplicar` valida ASCII y bytes (`reinsert.Ctx.enc`) y escribe el dump; `estado` |
| `misc_text.py` + `srwlib/fields.py` | Texto en campos fijos de hojas LICE (#49, #50): ayuda `IM/8` y `SHARED/8`, tutorial `IM/10`, BGM `IM/1`, terrenos `MPTI`, dudosos (`BATTLE/13`, `MAP/15`, `SHARED2/1`): `verify` (reescritura idéntica), `dump` → `work/dump/misc_text.jsonl` (3.379), `stats` |
| `db_text.py` + `srwlib/fixh.py` | Base de datos `Vers`+`Fixh`/`Bmsg` (#37): `IM/3/0`, `SHARED/3/0` (nombres `Strg` + párrafos `Expl`/`Sent`, regeneración idéntica), Shuffle Battler (`SHARED2/0/0`, `SBBATTLE/9/*`): `verify`, `dump` → `work/dump/db_text.jsonl` (18.202; `ctx` por tabla/columna), `stats` |
| ID build de la biblioteca (#40) | `reinsert.py check/leaves --ids --bloque db` (`id_db`): cada `S`/`E` de `IM`/`SHARED` (8.242 filas) = código de 3 caracteres (letras `ID_DB`, distintas de las del ELF); `build_test.py --texto-ids` ahora incluye ELF/overlays + biblioteca → `work/build/SRWOGG_ids_v3.iso` (md5 `39a4e45e…`) + `.ids.tsv` (3.641 + 8.242). `box_assign.py` usa la tabla de cada grabación (`box_catalog.ID_TAGS`: ids2–9 → v2, ids10–12 → v3) y `db_text.py dump` rellena `renderer`/`box_id` con `box_assign.tsv`. **Pendiente (mantenedor):** sesión `box_trace --watch --shots --tag ids10` con la v3 recorriendo la biblioteca (unidades, pilotos, armas, glosario, historia, Spirits, habilidades) y sortie/selección; luego `box_catalog.py`, `box_assign.py`, `db_text.py dump`, `fit.py`. Test: `tests/test_db_ids.py` |
| `box_trace.py --watch` (mejora) | Además de `samples` (tope de 8 por caja: en `ids10` 36 cajas llenas y solo 8 códigos de la biblioteca visibles) guarda `codes` = todas las cadenas de cada caja `{cadena: [veces, x, y]}` (hasta 4.000), leyendo solo las entradas **nuevas** del anillo (`read_new`) y avisando `¡posible desborde del registro!` (+ `<tag>.overruns.txt`) si se escribieron ≥ 90 % de las 1.024 entradas entre dos lecturas. `box_assign.py` y el aviso de códigos nuevos usan `codes`. Las grabaciones anteriores (`ids1`–`ids10`) no lo tienen: la sesión de la biblioteca hay que repetirla con el trace nuevo (`--tag ids11`, ya en `ID_TAGS`). Tests: `test_box_trace_hooks.Codes` |
| `box_assign.py` (derivación) | Las filas `S` de la biblioteca no vistas heredan la caja de su categoría (`ctx`): la de más lecturas entre las que recogieron ≥ 10 % (y ≥ 2) y con ≥ 3 filas vistas → `work/notes/box_assign_derived.tsv`; `db_text.py dump` las marca `caja-derivada`. Párrafos (`E`) no: su caja no se vio. Con `ids10`: 2.750 filas derivadas (10 categorías) → 2.880 con caja. Hay que confirmar con `ids11` |
| `elf_text.py` + `srwlib/mips_refs.py` | Cadenas del ELF y los 7 overlays (#13, #14, #39) con quién las apunta (pares `lui`+lo, `gp`, punteros en datos), huérfanas de tablas fijas: `scan`, `dump` → `work/dump/elf_text.jsonl` (7.832 entradas; nunca dentro de código: `code-ranges` → `work/notes/elf_code.tsv`, cuerpos de función de Ghidra), `refs ID` |
| `id_lookup.py` | (#30 paso 3, #38) ID build: cruza los registros de `box_trace` con `<iso>.ids.tsv` → `work/logs/id_lookup.tsv` (código → id, drawer, sitio, W/H/máx) y cobertura por módulo; `--codigo A3K` para un código leído en una captura |
| `memcard.py` | Juega una ISO de prueba con `work/emu/memcards/OGG_gamefaqs.ps2` en el slot 1 del PCSX2 del usuario: copia la card una vez (`SRWOGG_pruebas.ps2` en su carpeta de cards), cambia sólo `Slot1_Filename` y lo restaura al cerrar PCSX2 (respaldo `PCSX2.ini.srwogg.bak`). Lo usa `jugar_<iso>.py --memcard` |
| `ps2mc.py` | Memory card PS2 cruda (.ps2): listar, extraer y reescribir archivos con ECC recalculado |
| `ogg_save.py` | Saves de OG Gaiden ([`SAVES.md`](work/notes/SAVES.md)): descifra `OGA`/`OGAP` (XOR con tabla del ELF), edita y re-firma; `completo CARD SALIDA [N]` = Library completa + 280/144 cartas + 100 % + Clear Points 99.999.999 (probado en PCSX2) |
| `vmc_todas.py` | VMC única (formato OPL) con todas las partidas (`OGG_100` → `OGA`/`OGAP`, mantenedor → `OGAS00-02`, GameFAQs → `OGAS03-06`); requiere `mymcplus`; slots 04-06 sin verificar en el juego ([`SAVES.md`](work/notes/SAVES.md) §VMC) |
| `elf_plan.py` | Plan de reinserción del ELF y los overlays ([`REINSERCION_ELF.md`](work/notes/REINSERCION_ELF.md)): por cadena, capacidad estricta/segura, inglés estimado (glosario, mediana/p90; `en` real si existe), reubicable o no; `--tsv` |
| `img_extract.py` | (#31) **Todas** las imágenes de los contenedores gráficos → PNG indexado reversible ([`IMAGENES.md`](work/notes/IMAGENES.md)): P2IG + hojas crudas con CLUT (`GRAPHIC/0,1,2,8,12,14`, `GRAPHIC2/5`); `grupos`, `scan` (→ `work/notes/imagenes.tsv`, 6.873 hojas, 4.167 decodificadas), `export` (→ `work/extract/img/`), `sheets`, `png`, `pack RUTA PNG` (PNG → hoja del mismo formato), `check` (4.167/4.167 byte-idénticas). Tests sin ISO: `tests/test_img_extract.py` |
| `tex_dump.py` | Texturas P2IG y hojas crudas (`GRAPHIC/0` fondos 704×512, `GRAPHIC/8` tarjetas de capítulo) → PNG: `list` (`work/notes/texturas.tsv`), `sheets` (láminas de revisión en `work/logs/tex/`), `png RUTA` |
| `abrev_plan.py` | (#30) etiquetas `ABREV` del ELF/overlays: forma completa del glosario vs piso JP y capacidad → `work/logs/abrev_plan.tsv` (SEGURO / MEDIR / SIN-FORMA); `--aplicar` escribe las SEGURO en `elf_text.en.jsonl`; excepciones en `work/trans/abrev_full.csv` |
| `reinsert.py` | (#30) `en` de los dumps → hojas nuevas: script de historia (MAP/14, diálogo en 1 byte, rótulos en 2, `logo.build` + relectura), frases de batalla (1 byte, `phrases.build`, ranura 0xA800), campos fijos (`fields.apply`), base de datos (`Strg` + `Sent`/`Expl` regenerados), ELF + 7 overlays in situ (bloque `elf`, salida `ISO:<ruta>`, relectura de bytes y referencias, `--capacidad estricta|segura`; `--ids`: ID build, `id_elf`, tabla → `work/logs/elf_ids.tsv`); misc/db en texto mixto alineado a pares (`enc(pairs=True)`, `--dos-bytes` para comparar); límite = contenedor reconstruido (`lice._layout`): `check [--prueba]` (→ `work/logs/reinsert_check.log`, con las hojas que crecen), `leaves -o DIR`. Lo usa `build_test.py --texto`; `work/trans/elf_split.csv`: cadenas compartidas separadas por puntero (#105) |
| `text_drawers.py` | Cruza las cajas grabadas (`box_trace`) con los dumps: qué drawer dibuja cada tipo de texto (→ `work/logs/text_drawers.txt`) |
| `gs_lista.py` | Mide por PINE cuánto de las 2 listas de dibujo del GS (`0x1E40000`/`0x1EC0000`, 512 KiB c/u) usa el cuadro actual y cuenta glifos; detecta desborde (#92). |
| `free_watch.py` | Watchpoint en vivo por PINE (#42, #21): trampas en las 170 palabras libres de las medidas (`0x14E2D0–0x14E870`) y en el código muerto de `mem_dead.tsv`, canario en `0x690000–0x6C0000`, `sbrk` de newlib, comparación del código del parche y profundidad de la pila (`--solo-parche`: sin las zonas de #21; `--sin-trampas`: no escribe en código, sólo compara: las trampas escritas por PINE en código recompilado pueden tumbar PCSX2 o el juego, #33); `layout`, `watch` → `work/logs/free_watch/` (PCSX2 atiende **un** cliente PINE a la vez: otro cliente espera hasta que éste se cierre) |
| `text_inventory.py` | Inventario heurístico de texto SJIS: ELF + overlays (cadenas C) y cada grupo de hojas sin exigir terminador → `work/notes/inventario_texto.tsv` (resumen en `INVENTARIO_TEXTO.md`) |
| `tests/` | `test_srwlib` (round-trips), `test_font`, `test_fit` (las 5 medidas del ELF parcheado y la expansión de `fbe0_1b` en unicorn), `test_mmol` (splitter de MMOL en unicorn: JP igual que antes, inglés mixto bien partido) — `cd work/tools && python3 -m unittest discover -s tests` (unicorn: `pip install --user unicorn`) |

Configuración por máquina: `work/config.local.toml` (copia de `work/config.example.toml`, no versionado).

### Entorno
Instalado y verificado (detalle en [`work/notes/ENTORNO.md`](work/notes/ENTORNO.md)): PCSX2 v2.8.2 con BIOS
SCPH-90000 J v2.30 (JP: CRC `B0E44B93`), Ghidra 12.1.3 + plugin Emotion Engine
v2.1.38 (ELF importado y analizado en `work/ghidra_proj/OGGaiden`), JDK 21, armips 0.11 (nativo, compilado con cmake), ImHex
1.38.1, venv Python. En el servidor: X `:1` headless sobre la iGPU (Intel HD 530, `work/tools/xorg/xorg-1.conf`,
arranque con `sudo bash work/tools/xorg/start_x1.sh`; verificado con `glxinfo -B`: renderer Mesa Intel HD 530). Mientras
`weston.service` (usuario) tenga la GPU, el X `:1` no arranca (DRM master). Smoke con `SRW_DISPLAY=:1 SRW_RENDERER=12`
(OpenGL por hardware); por defecto sigue `:99` + renderer 13 (software, Xvfb).

### Material de glosario (`info/`)
62 páginas guardadas de **Akurasu Wiki**: 23 de *OG Gaiden* (OGG) y 39 de *OGs* (los `.1` son la página
equivalente del otro juego). Índice: [`work/glossary/INFO_INDEX.md`](work/glossary/INFO_INDEX.md). Las más
útiles: OGG *Pilot Database* (147 tablas), *Weapon List*, *Mech List*, *Pilot Spirit Database*, *Parts*,
*Pilot Skills*; OGs *English Patch* y *Menu Translations* (terminología del parche de Kingcom).

## 5. Detalle por frente (Fases 1–4)

La lista con prioridad está arriba, en «Próximos pasos». Esta sección guarda el detalle técnico de cada frente.

Regla: ningún extractor vale sin round-trip `unpack → pack` byte-idéntico y re-verificación de punteros.
Orden: C1 → C2 → (C3 ∥ C6) → C7 + fuentes → C4 → C5 → diseño de auditoría.

1. ~~C1 · #18~~ `srwlib` hecho (falta: extractores que llenen el JSONL, por formato).
2. ~~C2 · #15~~ índice hecho; queda abierto el formato interno de cada tipo de hoja (en sus issues).
3. **C3 · #16** script de `MAP.BIN`: **formato y extractor hechos** (round-trip idéntico, dump JSONL) y **probado en
   el juego desde el disco**: New Game → etapa 1 (`14/1`, 16 líneas) en inglés con VWF, 1–3 renglones, retratos y
   marcado `<…>` bien; savestate 3 (`14/34`) en vivo; **rótulos** de la etapa 1 (objetivos, condición de PP y su
   «None», aviso de ítems con color `#16…##`, nombre de lugar en el mapamundi) bien; qué instrucción dibuja cada
   rótulo: [`CONTENEDORES.md`](work/notes/CONTENEDORES.md); frases de derribo (tabla A) = caja de diálogo con
   retrato (en vivo, `logo_live.py` con `base =`). Falta: aviso de PP en pantalla, `#13` y `?`, y prueba en hardware. Y **C6 · #13/#14/#39** ELF + overlays en Ghidra (en paralelo).
4. ~~**C7 · #19**~~ **catálogo de renderers hecho** ([`work/notes/RENDERERS.md`](work/notes/RENDERERS.md)):
   4 drawers + 5 funciones de medida (todas en el ELF) + 13 envoltorios; los llamadores de menús están en
   W1OL. Los drawers SJIS **comprimen** el texto que supera el ancho máximo de su caja (dato del registro de
   layout → base de `fit.py`). **VWF en todos los drawers hecha en emulador (#22)**: 4 drawers + 5 medidas +
   espacio de cifra para rellenos numéricos (`RENDERERS.md` §7); falta probar en hardware.
   **#36 fuentes** — Hecho: fuente 18×18
   (formato, lookup, drawers, `font_tool.py`) y pruebas en ejecución con las partidas de GameFAQs
   (`FUENTES.md` §F2). Falta: renderer de la tarjeta de capítulo; quién usa `IM/0/0`; sprites con texto;
   spec de la fuente nueva. **PoC de VWF hecha y vista en el juego** (`work/patches/vwf.asm`, `font_build.py`,
   §F3); comparación de 16 fuentes OFL en §F4. **Fuente definitiva elegida**: Barlow Condensed Medium,
   altura x 8, `I` con remates (§F5c, `work/font/`, `font_retouch.py`): 63,4 car/línea. Diseño cerrado; falta
   llevar la VWF a los otros drawers.
5. ~~**C4 · #17**~~ frases de batalla: formato, extractor y dump hechos (`phrase_text.py`); queda #48 (piloto/situación
   por frase, contexto para traducir); el encoder en 1 byte está hecho (bloque `battle` de `reinsert.py`: tabla T, tope `0xA800`, 455 px, relectura; #42). ~~**C5 · #37**~~ base de
   datos: formato `Fixh`/`Bmsg` y extractor hechos (`db_text.py`); **todo el texto en cadenas tiene dump** (ayuda,
   tutorial, BGM, terrenos, ELF + overlays; ver [`INVENTARIO_TEXTO.md`](work/notes/INVENTARIO_TEXTO.md)). Falta: los
   reinsertores de cada bloque nuevo y el texto en texturas (#31, [`TEXTURAS.md`](work/notes/TEXTURAS.md)).
6. **C8 · #38** diseño de auditoría: ~~`fit.py` estático~~ (hecho, #24), ID build / build de calibración, traza runtime.
   **`fit.py` (#24) hecho**: mide igual que el juego parcheado (verificado ejecutando las medidas del ELF en
   unicorn) y valida contra `work/notes/renderers.toml`. Hallazgo: casi ninguna caja trae ancho máximo
   (`layout[4]` = −1) → el texto largo pisa al vecino; el límite es geométrico y hay que medirlo por caja
   (`box_trace.py`, RENDERERS.md §8). **#40 en curso**: grabación jugando (`box_trace.py --watch`) +
   `box_catalog.py` → `work/notes/boxes.gen.toml`: 2.089 cajas (sesiones `play`, `ids1`–`ids3`), 1.467 con límite
   (95 del juego, 1.219 por vecino, 153 medidas en captura) y 146 solo cifras; pendientes 476 (274 pista, 202 sin
   límite). Bordes de ventana por captura (`box_trace.py --shots` + `box_edges.py`): ayuda semiautomática.
   **`box_id` asignado** (`box_assign.py` → `work/notes/box_assign.tsv`, lo lee `elf_text.py dump`): 824 de 3.641
   cadenas del ELF/overlays con caja (607 con límite en px); 4 con el JP más ancho que el límite (columna `revisar`).
   Con la ID build el catálogo mide cada código como su JP (un vecino que viene detrás del texto ya no pasa por
   borde) y fusiona las sesiones en vez de pisarlas. Falta: diálogo sin retrato, MPOL/MMOL/SBOL/EXOL en juego.
   **Enfoque combinado (#59, decidido y hecho 2026-10-18)**: medidas reales → piso JP en `fit.py` para lo no medido
   (hecho) → `box_trace --shots` dirigido solo a lo que el piso rechaza (`work/logs/jp_floor.tsv`, 738 cadenas;
   empezar por el cartel de los 262 rótulos de lugar) → build de estrés en hardware antes del release. Ghidra/unicorn solo para escala W o `box_id`, no para límites ([`CAJAS_ENFOQUE.md`](work/notes/CAJAS_ENFOQUE.md)).
   **Frases de batalla (animación, `fbe0`)**: medidas con `build_test.py --phrases-regla` (regla de 63 dígitos / 63 «M»
   en todas las frases) y `shot_loop.py` (842 capturas): panel interior x 128–627, texto desde x = 150, **no se
   recorta** (pisa el marco hasta el borde de pantalla); calibración = `fit.measure`. Límite duro 477 px; propuesto
   **455 px** (margen simétrico), cargado como `battle.phrase` en `renderers.toml` y usado por `frases.py validar`.
   Misma caja para aliado y enemigo; el crítico no la cambia. Falta: la otra caja de frases (`FUN_001d5d30`, sin ver).
   **ISO de prueba para OPL** (`build_test.py`: JP + VWF + fuente nueva, mismo tamaño; con
   `--logo work/test/logo_14_01.toml --logo work/test/logo_14_34.toml` suma líneas del script en inglés):
   arranca y se ve bien en PCSX2 y **en PS2 real vía OPL** (2026-10-11, #22, #34).

**Traducción (Fase 3, adelantada):** primer borrador propio de las 43 hojas **integrado al
dump** (`lote.py estado`: 20.613/20.613); el inglés versionado vive en `work/trans/map_script.en.jsonl`. Flujo por
**tandas** con contexto acotado y validador de hablante/términos (`work/trans/PROMPT.md`, guía `STYLE.md`,
`lotes/REGISTRO.md`). Tanda = un nivel (hoja `14/N`), partida en dos en un corte de escena si pasa de 500 filas: 54
tandas en `work/trans/tandas.tsv`, **en orden de historia** (deducido de condiciones de victoria vs. `info/Flow_Chart`:
14/35 = esc. 4, ruta Tierra 14/9–13, ruta Espacio 14/36–40, 14/41 entre 14/15 y 14/16; a confirmar jugando, #64), más una
tanda `derribos` (las 60 líneas A que se repiten en todas las hojas). Cada tanda trae un brief: resumen de las 2
previas (`work/trans/contexto.tsv`, lo escribe cada sesión al terminar) + sus últimas líneas, reparto con
personalidad/voz/habla (`speakers.csv`: 84 voces cargadas el 2026-10-18) y términos de todos los glosarios con los
choques marcados. Todo lo hecho hasta acá se revisa con ese flujo (`Review batch: 14_05`), empezando por `derribos` y
`14_01` en orden. Aún no hay ningún resumen escrito (#65). Decisiones de término del 2026-10-18 (#44): 57 filas en
`decisiones.csv` (Aggressors, Villefort, Super Robot, Machine God Fist, Bartolle, Choukijin, ReEOTH Plan, Swordian,
Thugater, Valsion Custom, Neo DC, Gigi, Daizenger, Aegis Plan, Mumyo…), aplicadas al borrador; todos los choques de
`generado/reporte.txt` tienen decisión (lo que sigue listado son variantes viejas de `borrador_terminos.csv` y `ref/`).
También: 少尉 Lieutenant, Mizar the Great Quake / Alcor the Odd Quake, Retsu/Gou/Chou/Ten-class, Kenneth con el rango
de cada línea. En #44 solo queda `borrador_terminos.csv` (691 términos sin revisar). Lotes viejos con JP en git: #66.
**Frases de batalla:** las 157 tandas traducidas e integradas (`frases.py`; formato de entrada/salida y
reglas en su docstring): sin 「」 ni nombre, ≤ 2 líneas, ≤ 455 px (caja `battle.phrase`, medida, #40) y ≤ 63 B por línea.
Pendientes de información: #43 (fichas de voz), #46 (líneas a ver en el juego).

**Ahora (técnico, en orden):** **#42 watchpoint en vivo** (`free_watch.py`): primera sesión (2026-10-11, ISO
`SRWOGG_1b_e1v2.iso`, ~40 min, 2.228 muestras): **0 alertas** de ejecución o escritura sobre el espacio del parche;
pila del hilo principal: máx. 0x9900 de 0x80000 B. Falta una sesión que cubra combate, menús e intermisión. **#51**: en esa sesión la ventana 作戦目的 mostró rotos los objetivos de
victoria/derrota (fue con un savestate de otra ISO; falta reproducir con New Game). **#42 texto en 1 byte**: prototipo hecho y probado en unicorn (RENDERERS §9).
`fbe0_1b` (envoltorio en la entrada de `fbe0` que expande ASCII → SJIS en pila), `e700` mixto con VWF, `f5540`
`slti 2→1` y MMOL (15 palabras); `fit.lines(mixed=…)`/`font_build.encode(mixed=…)`. ISO de prueba
`work/build/SRWOGG_1b_e1v2.iso` (`build_test.py --phrases-1b --logo work/test/logo_14_01_1b.toml --logo
work/test/logo_14_34_1b.toml`). PCSX2 (v1): diálogo, backlog, frases y avance automático bien; los objetivos
(rótulos → `f6f0`, 2 bytes) salían rotos → 1 byte solo en líneas de diálogo (v2: objetivos bien, confirmado en PCSX2). Falta hardware
y una sesión más larga del watchpoint (combate, menús, intermisión). Después:
~~encoder definitivo de las frases~~ (hecho: `reinsert.py` bloque `battle`, con medida en px y relectura estricta de cada hoja; sintético: 341 hojas, −0x4ECD4 B, 0 avisos) y decidir MPTI (terreno, MPOL busca `@` de a 2 B).
**Reinsertor `reinsert.py` + `f6f0_1b` (#30)**: la base de datos, la ayuda, SBDB y los rótulos los dibuja sobre
todo `f6f0` (`text_drawers.py`), que leía de a 2 bytes; con `f6f0_1b` (envoltorio de expansión como `fbe0_1b`,
`e4e0` y `e3d0` mixtas; RENDERERS §10) todo eso puede ir en 1 byte. `w_16f620` copia el texto de a pares y corta en
`+ & # /`: el encoder deja cada control y el 00 en offset par. Prueba sintética: de 693 avisos (2 bytes) a 5
(límites de campo reales: sufijos de `BATTLE/13` de 2 letras, un terreno de 32 B); IM −0x123F8, SHARED −0x150A8,
SHARED2 −0x2400, SBBATTLE −0x9A7C, BATTLE −0x4EC98, ninguna hoja crece. ISO `work/build/SRWOGG_texto_prueba_v2.iso`
(`build_test.py --texto-prueba -o …_v2.iso`, md5 `fde16882…`; incluye los bloques script y battle). **Próximo (necesita al mantenedor)**: jugarla con New Game y
mirar base de datos, ayuda, tutorial, terrenos, Shuffle Battler, objetivos y combate (texto sintético
`<etiqueta> The quick brown fox…`). ~~#21 mapa de memoria~~ (verificado, ver abajo). **ISO v3c** (`SRWOGG_texto_prueba_v3c.iso`, md5 `f8ca8d05…`; la v3 y la v3b pisaban código y tablas de la fuente, no usarlas): igual que la v2 más el texto del ELF y los overlays en sintético de 1 byte → jugarla con New Game para ver qué pantallas leen texto del ELF/overlays y si alguna se rompe en 1 byte (→ lista de SJIS, REINSERCION_ELF §3.2); mirar especialmente la tarjeta de capítulo (#36). Sin el mantenedor: **reinserción del ELF y los overlays** según [`REINSERCION_ELF.md`](work/notes/REINSERCION_ELF.md) §4: ~~bloque `elf` in situ + relectura~~ (hecho) → ~~ID build~~ (hecha: `SRWOGG_ids_v2.iso`) → **sesión de `box_trace` con la ID build** (con el mantenedor: `python3 work/build/jugar_SRWOGG_ids_v2.py` + `box_trace.py --watch --tag ids1`, recorrer todas las pantallas; después `id_lookup.py`; REINSERCION_ELF §6.3) → pool. #21 cerrado: dos sesiones de `free_watch` con la v2 (mapa, combate, menús, guardado/carga, intermisión, Shuffle Battler) con 0 alertas → el hueco `0x690000–0x6C0000` queda verificado para el pool (escritura; el código muerto sólo para escritura, no ejecución: MEMORIA.md).

## 6. Índice de issues

Estado real (2026-10-26): **115 cerrados, 4 abiertos**. Lista viva: `gh issue list` en [`Nrlemo/srw-og-gaiden-en`](https://github.com/Nrlemo/srw-og-gaiden-en).

| Issue | Tema | Estado |
|---|---|---|
| #84 | Unit Stats: rótulos HP/EN duplicados (texto + sprite); vaciar el texto | abierto (`necesita-iso`) |
| #76 | Frases de batalla: revisar juegos de palabras, referencias y registros (130 JUEGO, 27 REF, 30 VOZ) | abierto (`apto-nube`) |
| #34 | D5 — plataformas objetivo y tester en hardware real / OPL | abierto (decisión) |
| #6 | Épica Fase 5 — build, validación y release | abierto |
| #1–#5 | Épicas de las fases 0–4 | cerradas |
| #100–#120 | Arreglos de capturas del mantenedor (f100–f120): abreviaturas, Funds, fichas, Parts, Weapon/Pilot Stats, Final Stage, icono de arma, pantalla de agradecimiento | cerrados |

Decisiones: D1 resuelta · D3 fuente propia · D4 resuelta (#29) · D6 #26, D7 #11, D8 #12 cerradas por decisión del mantenedor; D5 #34 sigue abierta.
Si un número de arriba no aparece en la tabla, está cerrado: consultarlo con `gh issue view <n>`.
