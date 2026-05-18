# TFG — Análisis Socioeconómico y Predicción de la Criminalidad Provincial en España

**Autor:** Eduardo López López · eduardo.lopez.contact@gmail.com  
**Tutor:** Alfonso Vegara  
**Grado en Business Analytics · Universidad Francisco de Vitoria · Curso 2025-26**

---

## Descripción

Este repositorio contiene el código del **Pilar 2 (Análisis del Dato)** del TFG:

> *Análisis socioeconómico y predicción de la criminalidad: una herramienta predictiva para la prevención proactiva del delito basada en indicadores socioeconómicos por provincias.*

El objetivo es doble: cuantificar el efecto de variables socioeconómicas sobre la criminalidad patrimonial en España (2010-2024) y construir modelos capaces de anticipar la tasa provincial de delitos a un año vista.

---

## Estructura del repositorio

| Archivo | Descripción |
|---|---|
| `TFGBAANALISIS3_MEJORADO.ipynb` | Notebook principal con los 5 modelos |
| `base_datos_maestra.csv` | Dataset maestro: 780 observaciones (52 provincias × 15 años) |

---

## Modelos implementados

| Modelo | Tipo | Propósito | R² test |
|---|---|---|---|
| **Naive (persistencia)** | Baseline | Benchmark de referencia | 0,879 |
| **Modelo A** | OLS pooled | Explicativo — efectos parciales entre provincias | -0,500 |
| **Modelo A'** | OLS efectos fijos provinciales | Explicativo — efectos within, corrige sesgo de panel | 0,719 |
| **Modelo B** | Regresión lineal + lag-1 | Predictivo — mejor equilibrio precisión/interpretabilidad | 0,876 |
| **Modelo C** | XGBoost + lag-1 | Predictivo — interpretabilidad SHAP/PDP | 0,845 |

**Conclusión central:** la criminalidad patrimonial provincial es genuinamente inercial. El baseline naive (persistencia pura) alcanza R²=0,879 en test, valor que ni el Modelo B (0,876) ni el Modelo C (0,845) logran superar. Las variables socioeconómicas no aportan capacidad predictiva incremental al horizonte de un año una vez conocido el valor anterior.

---

## Datos

- **Variable objetivo:** Tasa de delitos patrimoniales por 100.000 habitantes
- **Predictores:** Tasa de paro (EPA/INE), Renta neta media por persona (INE), Densidad de población, Dummy COVID (2020-2021)
- **Features temporales (Modelos B y C):** Lag-1 de la tasa de delitos, Δ Tasa de paro interanual
- **Fuentes:** Ministerio del Interior, INE (EPA, Atlas de Distribución de Renta, Padrón Continuo)
- **Periodo:** 2010-2024 · 52 provincias · 780 observaciones

---

## Cómo ejecutar

1. Abrir `TFGBAANALISIS3_MEJORADO.ipynb` en **Google Colab** o Jupyter.
2. Asegurarse de que `base_datos_maestra.csv` está en el mismo directorio de trabajo.
3. Ejecutar las celdas **en orden** — cada modelo depende de las variables definidas en celdas anteriores.

```bash
pip install pandas numpy scikit-learn xgboost linearmodels optuna matplotlib
```

---

## Validación

- **Partición temporal:** train 2010-2021 / test 2022-2024 (sin mezcla de fechas)
- **Walk-Forward Cross-Validation:** ventana expansiva, mínimo 4 años de entrenamiento por fold
- **Métricas:** RMSE, MAE, R², MAPE — calculadas en train, test y WF-CV
- **Benchmark obligatorio:** baseline naive de persistencia incluido en la comparación final

---

## Dashboard del Pilar 3

El sistema de alerta temprana basado en estos modelos está accesible en:  
🔗 [eduloopezzz.github.io/sistema-alerta-temprana-tfg](https://eduloopezzz.github.io/sistema-alerta-temprana-tfg)

---

## Licencia

Código bajo licencia MIT. Datos socioeconómicos de dominio público (INE, Ministerio del Interior).
