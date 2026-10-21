# Simuladores educativos de RAID y ZFS

Dos herramientas interactivas, cada una en un único archivo HTML, para entender cómo se distribuyen los datos y la paridad en distintos niveles de RAID y en los pools de ZFS, y qué pasa cuando fallan discos.

No necesitan instalación, servidor ni conexión a internet: se abren directamente en el navegador.

## Contenido

| Archivo | Descripción |
|---|---|
| `simulador-raid.html` | Simulador de niveles RAID clásicos: 0, 1, 4, 5, 6, 10, 50 y 60 |
| `simulador-zfs-raidz.html` | Simulador de pools ZFS: stripe, mirror, RAIDZ1/2/3, mirrors en stripe y combinaciones de vdevs |

## Cómo usarlos

1. Descargá o cloná el repositorio.
2. Abrí el archivo `.html` que quieras con cualquier navegador moderno.
3. Elegí el nivel o diseño, ajustá la cantidad de discos y el tamaño por disco.
4. Hacé clic en un disco para simular su falla. Otro clic lo reemplaza.

```bash
git clone https://github.com/<usuario>/<repositorio>.git
cd <repositorio>
# abrir simulador-raid.html o simulador-zfs-raidz.html en el navegador
```

## Simulador de RAID (`simulador-raid.html`)

Niveles incluidos: RAID 0, 1, 4, 5, 6, 10, 50 y 60.

- Diagrama de franjas con datos, paridad P, paridad Q y copias espejo.
- Cantidad de discos con mínimo y máximo según el nivel (RAID 10, 50 y 60 requieren cantidades pares).
- Simulación de fallas por disco, con estado del array: óptimo, degradado o perdido.
- Para RAID 10, 50 y 60 se evalúa la tolerancia por par o por grupo, no solo la cantidad total de discos caídos.
- Métricas: capacidad bruta y útil, eficiencia, tolerancia garantizada, mínimo de discos, lectura y penalización de escritura.
- Resumen de cada nivel con casos de uso, ventajas y desventajas.

## Simulador de ZFS (`simulador-zfs-raidz.html`)

Diseños incluidos: Stripe, Mirror, RAIDZ1, RAIDZ2, RAIDZ3, mirrors en stripe, 2 × RAIDZ1 y 2 × RAIDZ2.

- Diagrama de bloques de ancho variable, cada uno con su propia paridad (P, Q, R) y padding, tal como los reparte RAID-Z.
- Estados del pool en estilo ZFS: `ONLINE`, `DEGRADED` y `FAULTED`.
- Aviso de que la pérdida de un vdev implica la pérdida de todo el pool.
- Métricas: vdevs, IOPS aleatorios de lectura y escritura, lectura secuencial, capacidad útil aproximada y tolerancia.
- Generación del comando `zpool create` equivalente, con botón para copiarlo.

## Limitaciones

- Son simulaciones educativas: los valores de rendimiento (lectura, escritura, IOPS) son aproximaciones teóricas relativas a un solo disco, no mediciones reales.
- La capacidad útil de ZFS es aproximada: no descuenta el padding, el espacio reservado por ZFS ni la diferencia entre TB y TiB.
- Los nombres de dispositivo del comando generado (`sda`, `sdb`, etc.) son de ejemplo. En producción conviene usar rutas estables como `/dev/disk/by-id`.
- RAID no es un backup: ninguna de estas configuraciones protege contra borrados accidentales, ransomware o corrupción lógica.

## Publicarlos con GitHub Pages (opcional)

Para tener los simuladores online sin descargar nada:

1. En el repositorio, entrá a **Settings → Pages**.
2. En **Source**, elegí la rama `main` y la carpeta `/ (root)`.
3. Guardá. En unos minutos estarán disponibles en:
   - `https://<usuario>.github.io/<repositorio>/simulador-raid.html`
   - `https://<usuario>.github.io/<repositorio>/simulador-zfs-raidz.html`

## Estructura del repositorio

```
.
├── README.md
├── simulador-raid.html
└── simulador-zfs-raidz.html
```

## Tecnología

HTML, CSS y JavaScript sin dependencias ni librerías externas. Los estilos respetan el modo claro u oscuro del sistema.

## Licencia

Definí la licencia que prefieras (por ejemplo MIT) y agregá un archivo `LICENSE` al repositorio.
