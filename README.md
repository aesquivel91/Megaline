<p align="center">
  <img src="plans.jpg" alt="Megaline Analysis Banner" width="800">
</p>

# 📱 Megaline: ¿Cuál es la mejor tarifa?

## 🧭 Descripción del proyecto
Análisis comparativo de los planes **Surf** y **Ultimate** de la operadora **Megaline** para identificar **qué tarifa de prepago genera más ingresos mensuales** y cómo se comportan los usuarios en **llamadas, mensajes e internet**. El estudio combina **EDA**, construcción de **métricas de uso**, cálculo de **ingresos por usuario/mes**, y **pruebas de hipótesis** para respaldar recomendaciones de negocio.

---

## 🎯 Objetivos
1. Describir el comportamiento de uso por plan (llamadas, SMS, datos).  
2. Calcular el **ingreso mensual** por usuario y comparar entre planes.  
3. Evaluar estadísticamente si existen **diferencias significativas** en los ingresos entre **Surf** y **Ultimate**.  
4. Formular **recomendaciones** de marketing y de producto (pricing, upselling, dimensionamiento de red).

---

## 📚 Datos y diccionario
El proyecto utiliza tablas de consumo y catálogo de planes. Columnas clave inferidas del notebook:

| Columna | Descripción |
|---|---|
| `user_id` | Identificador del usuario |
| `city`, `region` | Ubicación del cliente |
| `reg_date`, `churn_date` | Alta y baja del servicio |
| `plan` | Plan contratado: **Surf**, **Ultimate** |
| **Llamadas** | `call_date`, `duration` (min) |
| **Mensajes** | `message_date` |
| **Internet** | `session_date`, `mb_used` |
| **Catálogo plan** | `minutes_included`, `messages_included`, `mb_per_month_included` |
| **Tarifas** | `usd_monthly_pay`, `usd_per_minute`, `usd_per_message`, `usd_per_gb` |
| **Métricas derivadas** | `minutes_per_month`, `minutes_over`, `gb_per_month`, `gb_over`, `revenue` |

> **Nota:** El notebook calcula consumo mensual por usuario (agregación por `user_id` y `month`) y luego **ingresos** con base en cuota fija + excedentes (min, SMS, GB).

---

## 🔍 Metodología
1. **Preparación**
   - Limpieza de nulos/duplicados, tipificación de fechas (`*_date`), generación de columna `month`.
   - Unión de tablas de uso con catálogo de planes.
2. **Métricas de uso**
   - Llamadas: minutos por mes y excedentes sobre `minutes_included`.
   - Mensajes: conteo mensual y excedentes sobre `messages_included`.
   - Internet: conversión `MB → GB`, GB por mes y excedentes sobre `mb_per_month_included`.
3. **Ingreso mensual**
   - `revenue = usd_monthly_pay + excedentes_min + excedentes_sms + excedentes_gb`.
4. **EDA**
   - Distribuciones y boxplots por plan para **llamadas, SMS, GB** e **ingresos**.
5. **Pruebas de hipótesis**
   - Comparación de **ingreso mensual** entre **Surf** y **Ultimate** (t‑test o Mann–Whitney según normalidad/varianzas).  
   - Nivel de significancia: `alpha = 0.05`.

---

## 📈 Hallazgos (según el notebook)
- **Uso**: *Ultimate* muestra patrones de consumo **más estables y predecibles**; *Surf* tiene mayor dispersión y presencia de **excedentes** en meses pico.  
- **Ingresos**: *Ultimate* presenta **ingresos promedio más altos** y menor varianza; *Surf* implica **mayor riesgo** (ingresos más variables) por excedentes, con **picos ocasionales**.  
- **Estadística**: las pruebas indican **diferencias significativas** en ingresos a favor de **Ultimate** (rechazo de H₀ al 5%).

> En conjunto, **Ultimate** genera **mayor ingreso medio** y **previsibilidad**; **Surf** puede atraer por precio pero depende de excedentes para elevar el ARPU.

---

## 🧠 Recomendaciones
- **Marketing/Comercial**:  
  - Promover **Ultimate** a usuarios de alto consumo (datos/voz) por su mayor ARPU y estabilidad.  
  - Mantener **Surf** como entrada de bajo costo con **estrategias de upselling** (bundles de GB/minutos, descuentos por migración a Ultimate).
- **Producto/Pricing**:  
  - Revisar precios de **excedentes** en Surf para reducir volatilidad de ingresos.  
  - Explorar **paquetes intermedios** que mitiguen “bill shock” en Surf.
- **Operación/Red**:  
  - Planificación de capacidad guiada por patrones de **GB_over** y **minutes_over** por región/mes.
  
---

## 🧪 Reproducibilidad

### Requisitos
- **Python 3.10+**  
- Paquetes: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `statsmodels`, `jupyter`

### Estructura sugerida del repo
```
megaline-tariffs/
├── data/                     # CSVs crudos y procesados (no versionar los pesados)
├── notebooks/
│   └── megaline.ipynb        # Notebook principal (este proyecto)
├── src/                      # Funciones auxiliares (cálculo de métricas, tests)
├── images/                   # Figuras exportadas
└── README.md
```

### Pasos de ejecución
```bash
# 1) Crear y activar entorno
python -m venv .venv
# macOS/Linux
source .venv/bin/activate
# Windows
.venv\Scripts\activate

# 2) Instalar dependencias
pip install -U pip
pip install pandas numpy matplotlib seaborn scipy statsmodels jupyter

# 3) Abrir el notebook
jupyter notebook notebooks/megaline.ipynb
```

> Si cuentas con un script de preparación, ejecuta `python src/prepare_data.py` para construir las tablas mensuales (`minutes_per_month`, `gb_per_month`, `revenue`).

---

## 📌 Definiciones clave
- **Excedente (overage)**: Consumo por encima de lo incluido en el plan (min/SMS/GB) facturado con `usd_per_*` según catálogo.  
- **ARPU**: Ingreso promedio por usuario y mes; métrica central para comparar planes.  
- **Significancia estadística**: Diferencias en ingresos se consideran reales si *p‑value* < 0.05.

---

## ✍️ Autor
**Andrés Esquivel Díaz** – *Data Analyst (Python · SQL · Tableau · Power BI)*  
LinkedIn: https://www.linkedin.com/in/andres-esquivel-diaz-08691337  
GitHub: https://github.com/aesquivel91
