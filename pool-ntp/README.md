# Servidores NTP publicos de pool.ntp.org

Lista actualizada a diario de las direcciones IP (IPv4) de los servidores de
hora publicos que forman parte de [pool.ntp.org](https://www.ntppool.org).

El proyecto pool.ntp.org no publica esa lista en ningun sitio: reparte los
servidores por DNS, unos pocos cada vez. Aqui se reconstruye consultandolo todos
los dias, se comprueba que cada servidor responda de verdad la hora, y se deja
el resultado en un fichero listo para usar.

Esta carpeta solo contiene esos ficheros. No hay codigo ni nada que
instalar: se descargan y se usan.

## Que hay aqui

| Fichero | Que es |
|---|---|
| `ntp-pool-v4.txt` | Una IP por linea, sin nada mas. Es el fichero que se carga en un firewall. |
| `ntp-pool-v4.csv` | Las mismas IPs con el nombre DNS que las sirvio. Comodo para abrirlo en una hoja de calculo. |
| `ntp-pool.json` | El detalle completo: totales, reparto por continente y pais, y ficha de cada servidor. |

Los tres se regeneran cada dia. Si un dia la lista no cambia, no se sube nada.

## Para que sirve

- Permitir o vigilar el trafico de hora (NTP) hacia servidores publicos
  conocidos, en vez de abrirlo a internet entero.
- Reconocer en los registros de red que una conexion iba a un servidor de hora
  del pool y no a un destino cualquiera.
- Tener a mano el mapa de que paises aportan servidores y cuantos.

## Como se usa

Para descargarlos o enlazarlos hace falta la direccion directa, que es la del
fichero con `raw.githubusercontent.com` delante. La forma mas comoda de
obtenerla: abrir el fichero aqui arriba y pulsar el boton **Raw**; la barra del
navegador ya muestra la direccion buena.

Los firewalls que admiten listas externas (EDL, feeds, objetos dinamicos)
apuntan a esa direccion del `.txt` y la releen solos cada cierto tiempo. Una vez
al dia sobra: la lista no cambia mas rapido que eso.

Quien no tenga esa opcion puede descargar el `.txt` y cargarlo a mano; sirve
igual, solo que hay que repetirlo de vez en cuando.

No hace falta cuenta ni permiso: el repositorio es publico y de solo lectura
para quien no lo mantiene.

## Lo que conviene saber antes de usarla

- **No esta completa, y no puede estarlo.** El pool entrega sus servidores por
  sorteo y da mas veces los mas rapidos, asi que los pequenos aparecen muy de
  tarde en tarde. La lista ronda el 85% de los servidores que el pool declara y
  va subiendo con los dias, pero nunca llega al 100%.
- **Estar en la lista no significa ser de fiar.** Cualquiera puede apuntar su
  servidor al pool. Esto dice "esta IP es un servidor de hora publico conocido",
  no "esta IP es segura".
- **Faltan los servidores de fabricante.** Los de Microsoft, Apple, Google,
  Ubuntu o los del propio operador de internet no pertenecen a pool.ntp.org y
  por tanto no salen aqui. Si se bloquea todo lo que no este en la lista, esos
  hay que permitirlos aparte.
- **Es una ayuda, no una defensa.** Lo que de verdad protege es tener unos pocos
  equipos internos encargados de la hora y que solo esos salgan a internet. Con
  eso montado, que a la lista le falte un servidor deja de importar.

## Historial

Cada cambio queda como un commit del dia, con el numero de servidores en el
mensaje. El historial sirve para ver como entra y sale gente del pool a lo largo
del tiempo. Nunca se reescribe.

## Origen

Los ficheros los genera un bot automatico, una vez al dia. Aqui solo llega el
resultado; el codigo que lo produce vive en otro repositorio.
