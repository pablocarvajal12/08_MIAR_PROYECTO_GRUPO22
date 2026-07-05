# Proyecto práctico — Aprendizaje por Refuerzo (Grupo 22)

Agente **DQN** para `SpaceInvaders-v0` (Atari) implementado con **Stable-Baselines3 0.10.0** y entrenado en Google Colab, con tres propuestas de mejora comparadas experimentalmente.

**Autores**:
- Vara Rodríguez, Jorge
- Larios Pérez, José María
- Fernández Gordo, Javier
- Carvajal Benítez, Pablo




## Contenido del repositorio

```
├── sb3_1_spaceinvaders_v0.ipynb   # Notebook completo (entrenamientos, test, gráficas, discusión y vídeos)
├── modelos/
│   └── dqn_spaceinvaders_v0_policy_latest.pt   # Pesos del mejor modelo en test (DQN base)
├── logs/                          # Logs de entrenamiento (CSV/JSON) y test de las 4 configuraciones
│   ├── sb3_1_spaceinvaders_v0/           # DQN base
│   ├── sb3_1_spaceinvaders_v0_double/    # Double DQN (incluye sus pesos .pt)
│   ├── sb3_1_spaceinvaders_v0_dueling/   # Dueling DQN (incluye sus pesos .pt)
│   └── sb3_1_spaceinvaders_v0_tuned/     # DQN con exploración ajustada (incluye sus pesos .pt)
└── videos/                        # Partidas grabadas del agente (también embebidas en el notebook)
```

## Configuraciones comparadas

| Configuración | Qué cambia respecto al base |
|---|---|
| **DQN base** | Red convolucional de Nature (Mnih et al., 2015), ε: 1.0→0.001 en el 30 % de 10M steps |
| **Double DQN** | Target con selección online / evaluación target (van Hasselt et al., 2016) |
| **Dueling DQN** | Cabeza en dos streams V(s) y A(s,a) (Wang et al., 2016) |
| **Exploración ajustada** | Annealing de ε en el 10 % del presupuesto (final 0.01), target update 5k, learning_starts 25k |

Protocolo común: misma semilla (42), preprocesado Atari estándar (84×84, 4 frames, reward clipping), criterio de parada = **100 episodios consecutivos con recompensa clipped > 20** (requisito del listado del grupo), y test final de **120 episodios con política determinista** sin clipping.

## Resultados

| Configuración | Steps hasta el criterio | Test: media | Test: std | Test: máx |
|---|---|---|---|---|
| DQN base | 2,63M | **650,6** | 182,0 | 1295 |
| Double DQN | 2,29M | 598,4 | **147,3** | 1280 |
| Dueling DQN | 2,21M | 622,0 | 171,0 | **1430** |
| Exploración ajustada | **1,01M** | 591,4 | 180,7 | 1400 |

Las tres mejoras alcanzan un rendimiento final comparable al base con hasta **2,6× menos interacciones** con el entorno. Discusión completa en la Parte 3 del notebook.

## Cómo ejecutarlo

1. Abrir el notebook en **Google Colab** con GPU (entrenado con T4).
2. Ejecutar la celda de instalación (`gym 0.17.3`, `atari-py`, `stable-baselines3 0.10.0`) y las celdas de definición en orden.
3. Ajustar `drive_root` (celda "3. Google Drive y rutas") a la carpeta de trabajo propia.
4. Los pesos `.pt` se cargan con la celda "Cargar policy aprendida" (o `build_model(...)` + `load_state_dict` para las variantes, ver celda M8).

Los vídeos del agente y todas las gráficas comparativas están embebidos en el notebook, por lo que puede revisarse sin ejecutar nada.
