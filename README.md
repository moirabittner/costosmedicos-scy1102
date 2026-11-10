# Modelo predictivo de costos médicos

Proyecto semestral de **SCY1102 – Gestión de Proyectos de Datos** (Duoc UC, 2026).
Caso 2: *Salud – costos médicos y bienestar del paciente*.

**Integrantes:** Moira Bittner · Nicolás Carrasco · Martín Corvalán · Martín Naranjo
**Docente:** Anibal Sotelo

Una aseguradora de salud privada quiere anticipar el costo médico anual de sus afiliados para planificar
su presupuesto y focalizar sus programas preventivos. Este repositorio reúne las tres evaluaciones del
proyecto: la factibilidad ética, financiera y de riesgos (EP1), el plan del proyecto (EP2) y la
implementación con su monitoreo y control (EP3).

## Estructura

```
├── data/
│   └── insurance.csv                      # 1.338 afiliados, 7 variables (Choi, 2018)
├── notebooks/
│   ├── EP1_Anexo_Verificacion.ipynb       # reproduce las cifras del informe de factibilidad
│   ├── EP2_Justificacion_Plan.ipynb       # respalda las cifras del plan de proyecto
│   └── EP3_Implementacion_Tecnica.ipynb   # preparación, modelado, evaluación y puerta de calidad
├── informes/
│   ├── EP1_Informe_Factibilidad.pdf
│   ├── EP2_Plan_de_Proyecto.pdf
│   ├── EP3_Informe_Implementacion_Monitoreo.pdf
│   └── EP3_Informe_Implementacion_Monitoreo.docx
├── presentaciones/
│   ├── EP1_Presentacion.pdf
│   └── EP2_Presentacion.pdf
└── requirements.txt
```

## Cómo ejecutar los notebooks

```bash
pip install -r requirements.txt
jupyter notebook notebooks/
```

Los notebooks leen el conjunto de datos desde `../data/insurance.csv`, así que hay que abrirlos desde la
carpeta `notebooks/`. Usan `random_state=42`, por lo que los resultados son reproducibles. El notebook de la
EP3 crea las carpetas `figuras/` y `salidas/` al ejecutarse.

> **Nota sobre versiones.** El Random Forest tiene un componente aleatorio que puede variar entre versiones
> de scikit-learn. Con otra versión, algunas cifras de ese modelo pueden cambiar en unos pocos dólares
> (por ejemplo, MAE de USD 2.608 en vez de USD 2.610). Las conclusiones no cambian. La regresión lineal es
> determinista y siempre da las mismas cifras.

## Resultados principales

| Evaluación | Resultado |
|---|---|
| **EP1 – Factibilidad** | El proyecto es técnicamente viable (Random Forest: R² 0,850, 71,8 % menos error que predecir el promedio), pero el error es 2,6 veces mayor en fumadores con la regresión lineal. Se recomienda aprobarlo con alcance restringido: planificación presupuestaria y prevención, nunca tarificación. |
| **EP2 – Plan** | PMBOK híbrido + CRISP-DM, requerimientos priorizados con MoSCoW, seis hitos (H1–H6) y la auditoría de equidad (diferencia de MAE entre subgrupos < 25 %) como condición para aprobar el hito H5. |
| **EP3 – Implementación** | Modelo final: regresión lineal **sin sexo ni región**, con la interacción fumador × obeso. R² 0,888 y MAE de USD 2.411 (74,7 % menos que la línea base). Todas las brechas de error entre subgrupos quedan bajo 25 % (14 % o menos en validación cruzada). |

El hallazgo que atraviesa todo el proyecto: **un R² alto no basta**. El primer modelo de la EP3 tenía un
R² de 0,890 y aun así se equivocaba un 64 % más con algunas regiones. Sacar la región, que funciona como
proxy socioeconómico, corrigió la brecha sin perder precisión.

## Gestión del proyecto

El seguimiento se hizo en Trello: plan por fases CRISP-DM, tareas T1 a T15 con responsable, fechas,
dependencias y checklist de avance, listas de estado, matriz de riesgos y reportes de estado.

## Limitaciones

Los datos son de Estados Unidos y no incluyen personas mayores de 65 años, y la variable objetivo es el
costo facturado, no la necesidad de salud. El modelo es un prototipo y no debe usarse en una cartera
chilena sin un conjunto de datos local. **Nunca debe usarse para tarificar, denegar cobertura o rechazar
afiliaciones.**

## Fuente de los datos

Choi, M. (2018). *Medical cost personal datasets* [Conjunto de datos]. Kaggle.
https://www.kaggle.com/datasets/mirichoi0218/insurance
