# Rambla SA · Perfiles de riesgo del inversor uruguayo y carteras modelo

**Proyecto Final · Propuesta C · Programación Analítica II**
Docente: Camila Palomeque · Segundo semestre 2026
Autor: Valentín Bisio

## Qué hace el proyecto

Rambla SA asigna el perfil de riesgo de sus clientes preguntándoles si se consideran conservadores, moderados o agresivos. Este proyecto compara **lo que los clientes dicen** (encuesta anual de perfil) con **lo que hacen** (registro de operaciones) para responder la pregunta del gerente de asesoramiento:

> ¿Cuántos clientes tienen una cartera que no se parece a lo que dijeron ser, y quiénes son?

El análisis se organiza en cinco etapas:

| Etapa | Contenido |
|---|---|
| 1. Análisis exploratorio y calidad | Carga con tipos y fechas, duplicados, faltantes, códigos de no respuesta, valores imposibles y estudio de quién no declara su patrimonio |
| 2. Creación de variables | Conversión de monedas a dólares, reconstrucción de la cartera de cada cliente, riesgo efectivo ponderado por monto y categorías de riesgo |
| 3. Análisis de negocio | Brecha entre perfil declarado y real, expectativas de retorno, dólares inmovilizados, una relación que se desarma al controlar e inconsistencias internas |
| 4. Visualización | Cinco gráficos con su conclusión (dos interactivos con Plotly) |
| 5. Perfiles y carteras modelo | Cinco perfiles nuevos basados en comportamiento y situación financiera, una cartera modelo por perfil con retorno y volatilidad, y propuesta de formulario |

### Principales resultados

- El **49,4%** de los clientes (313 de 633) tiene una cartera que no coincide con el perfil que declaró.
- **19 clientes** se declaran conservadores y tienen carteras agresivas (problema legal) y **44** se declaran agresivos y tienen carteras conservadoras (problema comercial).
- Los clientes que no declaran su patrimonio son los de **mayor patrimonio**: el faltante no es aleatorio.
- Los que esperan ganar más de 30% por año tienen carteras que rinden **7,4%** en dólares.
- Lo que explica el riesgo de la cartera es el **comportamiento** (frecuencia de monitoreo, tolerancia a pérdidas, fuente de información), no la autodeclaración ni las variables demográficas.

## Estructura del repositorio

```
proyecto-final-bisio/
├── README.md            <- este archivo
├── requirements.txt     <- librerías necesarias
├── .gitignore           <- archivos que no se suben al repositorio
├── datos/               <- los cuatro CSV originales (al 30/06/2026)
│   ├── encuesta_inversores.csv
│   ├── movimientos_cartera.csv
│   ├── catalogo_instrumentos.csv
│   └── tasas_referencia.csv
├── notebooks/
│   └── proyecto_final_rambla.ipynb   <- el análisis completo
└── salidas/             <- lo que genera el notebook al correrlo
```

## Cómo reproducir el análisis

### 1. Crear el entorno

Con Python 3.10 o superior, desde la carpeta del repositorio:

```bash
python -m venv .venv
source .venv/bin/activate        # en Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Correr el notebook

```bash
jupyter notebook notebooks/proyecto_final_rambla.ipynb
```

Dentro de Jupyter: **Kernel → Restart Kernel and Run All Cells**. El notebook corre de arriba abajo, en orden, sin pasos manuales: las etapas usan los resultados de las anteriores, así que no se deben correr celdas sueltas.

El notebook busca los datos en `../datos/` (la carpeta `datos` del repositorio), así que no hay que mover ningún archivo.

### Alternativa: Google Colab

1. Abrir `notebooks/proyecto_final_rambla.ipynb` en Colab.
2. Subir los cuatro CSV a una carpeta `datos` en el panel de archivos de Colab, o a `Mi unidad/Programación 2` en Google Drive (la primera celda monta el Drive).
3. **Entorno de ejecución → Ejecutar todas**.

## Salidas

Al correr el notebook se generan en `salidas/`:

| Archivo | Contenido |
|---|---|
| `encuesta_limpia.csv`, `movimientos_limpios.csv` | Las bases después de la Etapa 1 |
| `registro_decisiones.csv` | Cada decisión de limpieza con el porcentaje de la base afectado |
| `1_7_no_respuesta_por_quintil.png`, `2_3_riesgo_efectivo.png` | Gráficos de diagnóstico |
| `grafico_1_brecha.html`, `grafico_4_composicion.html` | Gráficos interactivos (Plotly) |
| `grafico_2_retorno_por_fuente.png`, `grafico_3_relacion_que_se_desarma.png`, `grafico_5_patrimonio.png` | Gráficos estáticos |
| `5_2_carteras_modelo.png` | Carteras modelo por perfil |

## Supuestos principales

- Todos los montos se convierten a dólares con el tipo de cambio (42,80) y el valor de la UI (6,55) al 30/06/2026.
- Se usan los montos invertidos (costo de compra), no el valor de mercado actual, que no está en los datos.
- Para pasar retornos en pesos a dólares se supone una devaluación igual al diferencial de inflación entre Uruguay (5,4%) y Estados Unidos (2,7%).
- La volatilidad de las carteras modelo se informa como un rango (correlación 0 y correlación 1), porque el catálogo no trae las correlaciones entre instrumentos.
