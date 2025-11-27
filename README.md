# Sistemas de Control Inteligente Aplicados a la Gestión Térmica de Edificios

Este repositorio contiene la implementación y el análisis comparativo de diversas estrategias de control inteligente y adaptativo aplicadas al problema de la **asignación dinámica de potencia térmica** para mantener la temperatura deseada en un edificio de cuatro salones acoplados.

El proyecto busca evaluar el compromiso entre **confort térmico (velocidad y precisión)** y **eficiencia energética (consumo de potencia)** en sistemas complejos.

## 📁 Estructura del Repositorio

```
sistemas_control_inteligente/
├── Comparativa.ipynb           # Notebook principal de análisis y gráficas comparativas.
├── controllers/                # Notebooks con la lógica detallada de cada controlador.
│   ├── Controlador_EGT.ipynb   # Teoría de Juegos Evolutiva (EGT)
│   ├── Controlador_ESC.ipynb   # Extremum Seeking Control (ESC)
│   └── Controlador_MPC.ipynb   # Model Predictive Control (MPC)
├── data/                       # Archivos de resultados de simulación (.csv) para la comparativa.
│   ├── datos_simulacion_EGT.csv
│   ├── datos_simulacion_ESC.csv
│   └── datos_simulacion_MPC.csv
├── deliveries/                 # Entregas originales del proyecto.
│   ├── Entrega 1 SDCI.ipynb    # Incluye Control On/Off y Lógica Difusa.
│   └── Entrega 2 SDCI.ipynb    # (Versión anterior de los controladores principales)
└── README.md                   # Este archivo.
```

## 🧠 Controladores Implementados y Analizados

El proyecto evalúa cinco estrategias distintas, categorizadas por su complejidad y enfoque:

| Estrategia | Enfoque | Archivo de Origen | Complejidad |
| :--- | :--- | :--- | :--- |
| **Control ON/OFF** | Clásico (Umbral) | `deliveries/Entrega 1 SDCI.ipynb` | Baja |
| **Lógica Difusa** | Control Heurístico (Reglas) | `deliveries/Entrega 1 SDCI.ipynb` | Media |
| **EGT (Teoría de Juegos Evolutiva)** | Descentralizado, Reactivo, Asignación de Recursos | `controllers/Controlador_EGT.ipynb` | Alta |
| **MPC (Control Predictivo por Modelo)** | Optimización, Proactivo, Seguimiento de Trayectoria | `controllers/Controlador_MPC.ipynb` | Alta |
| **ESC (Extremum Seeking Control)** | Adaptativo, Búsqueda de Óptimo en Tiempo Real | `controllers/Controlador_ESC.ipynb` | Media/Alta |

## 📊 Análisis Comparativo de Desempeño

El análisis se centra en las métricas de desempeño clave obtenidas de las simulaciones.

| Métrica | MPC | EGT | ESC (Un salón) |
| :--- | :--- | :--- | :--- |
| **Tiempo Ejecución ($\text{ms}$ / $\text{s}$)** | 35.7 s | **493 ms** | 211 ms |
| **Energía Total (1h) [MJ]** | 24.73 MJ | **22.32 MJ** | 6.23 MJ |
| **$t_{est}$ Promedio [min]** | **5.8 min** | 45.2 min | 33.68 min (Salón 5) |
| **$\text{E}_{ss}$ Promedio [°C]** | $-0.19 \text{ °C}$ | **$-0.01 \text{ °C}$** | $-0.0559 \text{ °C}$ (Salón 5) |
| **$\text{Amp}_{osc}$ Promedio [°C]** | $0.00 \text{ °C}$ | **$0.0000 \text{ °C}$** | $0.0330 \text{ °C}$ (Salón 5) |

### Conclusiones Principales:

*   **Velocidad (Ts):** El **MPC** domina al alcanzar el estado de referencia en menos de 6 minutos, gracias a su naturaleza proactiva y horizonte de predicción.
*   **Precisión ($\text{E}_{ss}$):** El **EGT** demuestra la mayor precisión en estado estacionario ($-0.01 \text{ °C}$), un éxito directo de su función de pago asimétrica diseñada para eliminar el error de seguimiento.
*   **Eficiencia Energética:** La estimación de consumo energético total muestra que el **EGT** es el más eficiente en la simulación de 4 salones ($22.32 \text{ MJ}$). El **ESC** (escalado a 4 salones con $\approx 24.92 \text{ MJ}$) y el **MPC** ($24.73 \text{ MJ}$) muestran una eficiencia similar y son competitivos.
*   **Consumo Computacional:** El **EGT** ($\mathbf{493 \text{ ms}}$) y el **ESC** (escalado a 4 salones con $\approx $\mathbf{844 \text{ ms}}$) son extremadamente rápidos y eficientes computacionalmente. El **MPC** ($35.7 \text{ s}$) es significativamente más lento debido a la resolución de un problema de optimización cuadrática en cada paso de control, lo que subraya la compensación entre velocidad de ejecución y capacidad predictiva.

## 🚀 Uso del Repositorio

Para replicar los resultados y el análisis:

1.  **Clonar el Repositorio:**
    ```bash
    git clone https://github.com/rich-coding/sistemas_control_inteligente.git
    ```
2.  **Instalar Dependencias:** Los controladores MPC y EGT requieren librerías de optimización (`cvxpy` con solver `OSQP`) y las básicas (`numpy`, `matplotlib`, `pandas`).
3.  **Ejecutar Simulaciones:** Ejecute los notebooks dentro de la carpeta `controllers/` para generar los archivos `.csv` en la carpeta `data/`.
4.  **Ejecutar Comparativa:** Abra y ejecute `Comparativa.ipynb` para cargar los datos y generar las gráficas de comparación.
