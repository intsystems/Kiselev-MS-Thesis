# Code

Этот каталог содержит исходный код вычислительных экспериментов магистерской ВКР (глава 4). Код состоит из двух независимых частей, соответствующих двум подразделам главы.

```
code/
├── mlp_mnist/      # §4.1 -- полносвязная нейронная сеть на MNIST
├── transformer/    # §4.2 -- трансформерная языковая модель nanochat
└── shared/         # общие утилиты (загрузка данных, стиль графиков) для mlp_mnist
```

## Установка

Клонировать репозиторий:
```bash
git clone https://github.com/intsystems/Kiselev-MS-Thesis.git
cd Kiselev-MS-Thesis/code
```

## `mlp_mnist/` -- эксперимент на полносвязной сети

Реализация на чистом `PyTorch`, без сторонних зависимостей сверх стандартного стека.

**Установка зависимостей:**
```bash
pip install -r requirements.txt
```

**Запуск (из каталога `mlp_mnist/`):**
```bash
python run_experiments.py             # вычисление Δ_1, Δ_2, Δ_2^(D) для сетки k
python visualize.py                   # построение графика landscape_convergence.pdf
python compute_spectrum_data.py       # спектр матрицы Гессе
python plot_spectrum.py               # график hessian_spectrum.pdf
python compute_surface_data.py        # двумерные сечения функции потерь
python plot_surface_2d.py             # график loss_surface_2d.pdf
```

Каждый `run_*`/`compute_*` пишет JSON-лог, который потребляется соответствующим `plot_*`/`visualize.py` для построения PDF. Конфигурация эксперимента -- в `config.yaml`.

## `transformer/` -- эксперимент на nanochat (NeurIPS submission)

Полностью соответствует репозиторию, сопровождавшему статью «Curvature-Aligned Probing for Local Loss-Landscape Stabilization» (препринт, NeurIPS 2026). Установка через `uv` и `pyproject.toml`:

```bash
cd transformer
uv sync
```

Запуск через bash-скрипты в `transformer/configs/`:
```bash
bash configs/run_fig2_fig3.sh     # отношение Δ_2^(D)/Δ_2
bash configs/run_fig4.sh          # область применимости квадратичной аппроксимации
bash configs/run_fig5_fig6.sh     # сравнение алгоритмов оценивания (S-curve, heatmap)
bash configs/run_tab2.sh          # таблица времен трех алгоритмов
```

Эксперименты подгружают чекпоинт `nanochat d6` шага 3500 из `~/.cache/nanochat/` (переопределяется через `NANOCHAT_BASE_DIR`). Все вычисления выполняются в float32 без автокаста; SDPA math-ядро используется для численной устойчивости спектральных вычислений. Подробности и карта «эксперимент -> скрипт -> график» -- в `transformer/README.md`.

## Соответствие фигур и таблиц диссертации

| Артефакт ВКР | Код |
|---|---|
| Рис. `landscape_convergence` | `mlp_mnist/run_experiments.py` -> `visualize.py` |
| Рис. `hessian_spectrum` | `mlp_mnist/compute_spectrum_data.py` -> `plot_spectrum.py` |
| Рис. `loss_surface_2d` | `mlp_mnist/compute_surface_data.py` -> `plot_surface_2d.py` |
| Таблица `tab:landscape-exponents` | `mlp_mnist/run_experiments.py` (постобработка) |
| Рис. `exp-subspace-vs-full` | `transformer/experiments/fig3_subspace_ratio/` |
| Рис. `exp-quadratic-validity` | `transformer/experiments/fig4_quadratic_sigma/` |
| Таблица `tab:exp-runtimes` | `transformer/experiments/tab2_estimator_phases/` |
