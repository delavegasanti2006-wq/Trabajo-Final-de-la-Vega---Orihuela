# Proyecto Final B · Arroyo Capital

Tesis de inversión y portafolio en la industria del cine.
Programación Analítica II · Universidad de Montevideo · S2 2026 · Docente: Camila Palomeque

**Integrantes:** _Nombre Apellido_ y _Nombre Apellido_

## Qué hace el proyecto

Arroyo Capital, un fondo de private equity ficticio, va a colocar **USD 500 millones** en producción cinematográfica.
El proyecto responde, con 40 años de datos de películas (1980-2020), qué clase de película tiene mejor relación entre riesgo y retorno
y cuánto se puede perder. El análisis sigue las cinco etapas de la letra:

1. Auditoría y limpieza de la base (duplicados, centinelas de faltante, signos invertidos, presupuestos en otra escala, categorías mal escritas, faltantes no aleatorios).
2. Variables nuevas: dólares constantes de 2020 con el IPC de EE.UU., retorno (recaudación / presupuesto) y regla del 2,5x.
3. Análisis de negocio: crecimiento real, retorno por segmento, probabilidad de perder plata y peor decil, segmentos dominantes.
4. Visualizaciones (incluye un gráfico interactivo con Plotly).
5. Tesis de inversión: cinco arquetipos, asignación de los USD 500 M, simulación del portafolio y prueba fuera de muestra (criterios hasta 2014, verificación 2015-2020).

## Estructura del repositorio

```
proyecto-final-apellido1-apellido2/
├── README.md
├── requirements.txt
├── .gitignore
├── datos/
│   ├── peliculas.csv        # base histórica (sin modificar)
│   └── ipc_eeuu.csv         # IPC de EE.UU., base 1982-1984 = 100
├── notebooks/
│   └── proyecto_final_arroyo_capital.ipynb   # todo el análisis, de arriba hacia abajo
└── salidas/                 # figuras, tablas y gráfico interactivo que genera el notebook
```

## Cómo reproducirlo

Requiere Python 3.10 o superior.

```bash
# 1. Clonar el repositorio y entrar a la carpeta
git clone <url-del-repositorio>
cd proyecto-final-apellido1-apellido2

# 2. Crear y activar el entorno
python -m venv .venv
source .venv/bin/activate        # en Windows: .venv\Scripts\activate

# 3. Instalar las dependencias
pip install -r requirements.txt

# 4. Abrir el notebook
jupyter notebook notebooks/proyecto_final_arroyo_capital.ipynb
```

Después, en Jupyter: **Kernel > Restart & Run All**. El notebook corre de arriba hacia abajo sin errores y tarda menos de un minuto.
Las rutas son relativas (`../datos`), así que hay que abrirlo desde la carpeta `notebooks/` o desde la raíz del repo.

**En Google Colab:** subir las carpetas `datos/` y `notebooks/` a `/content/`, abrir el notebook y ejecutar todas las celdas
(el notebook busca los archivos en `../datos`, `datos` y `/content/datos`).

## Orden de lectura del notebook

| Sección | Contenido |
|---|---|
| 0 | Configuración y constantes del análisis |
| 1.1 a 1.9 | Carga, estructura, duplicados, faltantes, valores imposibles, escala del presupuesto, categorías, cobertura temporal y decisión sobre faltantes |
| 2.1 a 2.3 | Dólares constantes, retorno, regla del 2,5x y variables adicionales. Bitácora de cambios |
| 3.1 a 3.3 | Crecimiento, retorno por género/clasificación/origen/franquicia, IMDb y riesgo por segmento |
| 4.2 a 4.5 | Distribución del retorno, comparación de géneros, presupuesto contra recaudación y plano de riesgo (Plotly) |
| 5.1 a 5.6 | Arquetipos, asignación, simulación, alternativas, fuera de muestra, qué no hacer y limitaciones |
| Cierre | Cifras clave con su sección de origen |

## Salidas generadas (`salidas/`)

| Archivo | Qué es |
|---|---|
| `01_evolucion_nominal_vs_real.png` | Recaudación mediana por año, corriente contra constante |
| `02_mapa_calor_correlaciones.png` | Correlaciones (Spearman) |
| `03_distribucion_retorno.png` | Distribución del retorno en escala log |
| `04_retorno_por_genero.png` | Mediana y promedio por género |
| `05_presupuesto_vs_recaudacion.png` | Hexbin presupuesto contra recaudación |
| `06_riesgo_retorno_interactivo.html` | Gráfico interactivo Plotly (abrir en el navegador) |
| `07_fuera_de_muestra.png` | Entrenamiento contra prueba |
| `bitacora_limpieza.csv` | Cada corrección con el % de la base afectado |
| `segmentos_riesgo_retorno.csv` | Riesgo y retorno de los 49 segmentos |
| `asignacion_fondo.csv` | Asignación propuesta de los USD 500 M |
| `cifras_clave.csv` | Números de la presentación y la sección donde nacen |

## Decisiones principales

- **Presupuestos en miles:** 48 películas con presupuesto menor a USD 1 M y retorno mayor a 100x se multiplican por 1.000.
- **Faltantes:** el 28,9% de las películas no tiene presupuesto y falta de forma no aleatoria; se excluyen del cálculo de retorno y se corrige con ponderadores como escenario de robustez.
- **Métrica de riesgo:** probabilidad de no llegar a 2,5x y retorno del peor decil, no el promedio.
- **Simulación:** 20.000 portafolios con semilla fija (`SEMILLA = 42`), por lo que los resultados son reproducibles.

## Datos

Basado en el dataset público de Kaggle `danielgrijalvas/movies`, con problemas de calidad agregados para uso docente. Arroyo Capital es un fondo ficticio.
IPC: U.S. Bureau of Labor Statistics, serie CPI-U.
