# TP4 – CNN en Skin Dataset (ISIC)

**Redes Neuronales – ITBA.** Clasificación de lesiones de piel en 9 clases (dataset ISIC, ~880 imágenes) con redes convolucionales, comparadas contra una MLP.

## Contenido
- `CNN_en_Skin_Dataset.ipynb`: notebook **ejecutado**, con los outputs, el análisis y la declaración de uso de IA.
- `requirements.txt`: dependencias.
- El dataset **no** está incluido (ver abajo).

## Cómo reproducirlo
1. Clonar el repo y crear un entorno:
   ```bash
   python -m venv .venv
   source .venv/bin/activate        # en Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```
2. Copiar el dataset en `data/Split_smol/`, con la estructura `train/<clase>/` y `val/<clase>/`, tal como lo distribuye la cátedra.
3. Ejecutar el notebook de arriba hacia abajo **desde la carpeta que contiene `data/`**:
   ```bash
   jupyter notebook CNN_en_Skin_Dataset.ipynb
   ```

No se modificó ninguna constante de la sección 0 (semillas `[0, 1, 2]`, `SEED_SPLIT = 42`, 60 épocas máximas con paciencia 10, batch 32, 2 copias aumentadas). La ejecución completa corre en CPU; el tiempo depende del procesador (la cátedra estima 15–20 minutos en una CPU de 20 núcleos).

> **Windows: rutas largas.** Algunas imágenes de la clase *Tinea Ringworm Candidiasis* tienen nombres de más de 160 caracteres. Si la carpeta del proyecto está en una ruta larga (por ejemplo dentro de `Escritorio\...`), la ruta completa supera el límite de 260 caracteres de Windows y la carga falla con `FileNotFoundError`. Hay dos soluciones: ubicar el proyecto en una ruta corta (por ejemplo `C:\TP4\`) o habilitar `LongPathsEnabled` en el registro de Windows.

## Metodología (resumen)
- Eliminación de 34 imágenes duplicadas (33 repetidas entre `train/` y `val/`) y nuevo split estratificado 60/20/20 con semilla fija: train 506, val 169, test 169.
- Normalización por canal con estadísticas calculadas solo con train.
- Data augmentation hecha a mano, aplicada solo a train y después del split.
- Comparaciones con 3 semillas (media ± desvío); el test se usa una sola vez.

## Resultados principales (test, 5 semillas)
| Modelo | Parámetros | Accuracy | F1 macro |
|---|---|---|---|
| CNN final (3 bloques, 8 filtros, 64 px, flips) | 269.521 | 0.671 ± 0.025 | 0.661 ± 0.029 |
| MLP 64 px | 6.358.793 | 0.496 ± 0.054 | 0.481 ± 0.056 |

El análisis completo, las limitaciones (dataset chico, sesgo de fuente entre clases, una sola partición) y la declaración de uso de IA están en el notebook.
