# Zombie-nvenidos v0.79 — Hospital, etapa 2

Las versiones **Online_Render_PWA** y **Offline_HTML** incluyen las mismas colisiones y reglas de juego. El modo cooperativo utiliza un servidor Node.js con WebSocket.

## Cómo publicar la versión online

1. Subir el **contenido**, no la carpeta contenedora, de `Online_Render_PWA/` al repositorio GitHub de Render.
2. Configurar Render como Web Service Node, con `npm install` y `npm start`.
3. Cerrar y volver a abrir la PWA instalada después de que Render publique la actualización. En Android se instala desde Chrome; en iPhone se añade a la pantalla de inicio desde Safari.

Para jugar sin conexión, abrir `Offline_HTML/Zombie-nvenidos_v0.79_Hospital_Etapa2_OFFLINE.html` en un navegador. El HTML es independiente.

## Hospital — colisiones y objetos

- **41 huellas de colisión individuales** en camas, armarios, gabinetes, mesas, bancos, macetas y aparatos. El pasillo y las áreas libres de las salas quedan transitables.
- **21 fuentes de botín dentro** de muebles, cajoneras, estanterías y armarios.
- **10 fuentes de botín sobre superficies** (camas, mesas, camillas y escritorios). Se puede usar la tecla E desde el suelo contiguo sin atravesar el objeto.
- **16 aparatos golpeables**: computadoras, monitores, equipamiento radiológico y de laboratorio, máquinas expendedoras y electrodomésticos. Tras 2 golpes acertados pueden soltar **Cables (44), Componentes eléctricos (37), Cobre (43) y/o Hierro (42)**, según su tipo.
- El botín aleatorio se **distribuye una única vez por partida, al ingresar por primera vez al hospital**. Salir, volver a entrar, cargar la partida o abrir muebles vacíos no duplica existencias.
- Se mantienen **2 zombis por habitación** a la misma altura y separados, más **6 en el pasillo**. La generación inicial verifica el suelo transitables y no ocupa muebles.

## Portones

- En los dos portones del hospital, el margen interior de proximidad para autorizar el cierre de personajes y zombis se reduce aproximadamente a la mitad; se sigue bloqueando el cierre si alguien ocupa el propio riel, y los vehículos mantienen su volumen completo de colisión.
- En los dos portones de la estación de policía se incorpora la **reapertura inmediata al detectar jugador, zombi o vehículo durante el cierre**; la comprobación también tiene en cuenta la posición de los invitados conectados cuando la procesa el anfitrión.

## Cooperativo y prevención de duplicaciones

- El servidor autoriza por orden de llegada cada recogida del hospital (individual o «Recoger todo»).
- Los impactos a aparatos del hospital se procesan en el servidor, que valida la proximidad, la sala y el estado del equipo. Al destruirlo, los materiales se generan **una única vez** para todos.
- Los materiales resultantes llevan identificador único y el servidor solo acepta **una reclamación** por objeto.
- El inventario, las llaves, las municiones y la durabilidad siguen siendo individuales; el botín, el estado de los equipos y las puertas pertenecen al mundo compartido.

### Compatibilidad con partidas anteriores

Los muebles de una partida anterior conservan su contenido y vaciado. Las **10 fuentes nuevas de objetos sobre superficies** se incorporan vacías a las partidas antiguas que ya tenían el hospital inicializado; esto evita generar botín adicional y duplicaciones. Para recibir la nueva distribución aleatoria de primera visita, empezá una partida nueva o importá una copia que no haya ingresado nunca al hospital.

### Validación efectuada

- Sintaxis JavaScript del cliente (offline/online) y del servidor Node: correcta.
- Prueba de navegador Chromium (almacenamiento simulado): 31 fuentes accesibles, 26 zombis iniciales en suelo válido, botín no regenerado tras salir/entrar, aparatos destruidos tras dos golpes con materiales en el suelo.
- Prueba local del servidor con clientes simulados: primera recogida gana; los impactos en aparato único solo producen una destrucción; un material caído solo se reclama una vez.
- Portones: hospital permite cerrar desde más cerca sin ignorar ocupación del riel; comisaría reabre ante ocupación durante el cierre.

**Limitación de verificación:** estas pruebas no sustituyen una partida larga simultánea entre varios celulares en Render. Hacé una copia de seguridad y probá los accesos, muebles y aparatos con dos dispositivos antes de continuar una partida importante.
