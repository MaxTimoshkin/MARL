# MARL
**Применение многоагентных методов обучения с подкреплением для балансировки пуассоновских потоков.**

Задается модельный граф $G=\langle V,E \rangle$. 
* Для изменения модельного графа можно изменить множества $V, E$. 
* Для визуализации графа используется также список позиций вершин $\textit{positions}$.
* Для визуализации нового графа можно запустить nx.draw(G, pos, with_labels=True).

Далее по заданному графу $G$ 
* для каждого коммутатора автоматически формируется множество выходных портов $E_v$.
* формируются множества путей без циклов для каждого потока paths[v_src][v_dst].

Можно также изменить: 
* Параметр интенсивности обслуживания каждого выходного порта коммутатора $μ$.
* Максимальный размер очереди $S$.
* По умолчанию: $μ = 1, S = 5$.

Параметры интенсивности каждого потока λ[v_src][v_dst] задаются случайным образом.
* При желании данную матрицу можно заполнить другими значениями.

Может быть изменен максимальный размер памяти воспроизведения (replay buffer) в следующей строке:
* exp_replays = [ReplayBuffer(2000) for v in V]
* По умолчанию: 2000.

Может быть изменена скорость обучения (learning rate) каждого агента в следующей строке:
* optimizers = [torch.optim.Adam(agents[v].parameters(), lr=1e-4) for v in V]
* По умолчанию: 1e-4.

Могут быть изменены гиперпараметры обучения метода DQN:
* batch_size (размер батча) = 256
* total_steps (общее количество шагов обучения) = 50_001
* decay_steps (количество шагов уменьшения параметра epsilon-жадной стратегии epsilon) = 30_000
* init_epsilon (начальное значение epsilon) = 1
* final_epsilon (конечное значение epsilon после decay_steps шагов) = 0.02
* loss_freq (частота охранения значений функции потерь агентов) = 400
* refresh_target_network_freq (частота обновления целевой нейронной сети очередного агента) = 100
* eval_freq (частота оценки качества работы алгоритма) = 400
* max_grad_norm (максимальная норма градиента) = 50

В разделе "**Создание валидационной выборки для оценки качества**"
* Может быть изменено общее количество временных шагов $T$ на валидационной выборке потоков.
* $t = \overline{0..T}$
* По умолчанию: $T = 100$

В разделе "**Обучение**" производится обучение нейронных сетей агентов и сохранение обученнх моделей.

В разделе "**Построение графиков зависимостей $r(t)$, $Loss(t)$**" после обучения могут быть построены соответствующие графики зависимостей:
* График зависимости масштабированной награды от времени $r_scale(t)$.
* График зависимостей функций потерь каждого агента от времени $Loss_v(t)$.
* График зависимостей функций потерь каждого агента от времени в логарифмическом масштабе $\log Loss_v(t)$.

В разделе "**Визуализация графа сети**" для визуализации сети может быть использовано средство визуализации, реализованное как функция visualize(G, N, selected_thread=None)
* Данная функция визуализирует текущее состояние сети. 
* Принимает на вход граф $G$, информацию о поступающих потоках $N$.
* Существует  возможность выделить маршрут отдельного потока selected_thread=[v_src, v_dst], передав его третьим аргументом.

В разделе "**Тестирование и анализ обученных агентов на новой выборке потоков**"
* можно загрузить сохраненные обученные модели.
* на сформированной валидационной выборке посмотреть построенные маршруты для каждого потока в валидационной выборке потоков.

В разделе "**Анализ**" для любого момента времени $t$ на валидационной выборке потоков можно
* отобразить все возможные альтернативные маршруты и рассчитать награды, которые можно было получить при выборе каждого маршрута.
* рассчитать значение недополученной награды (разность между максимально-возможной наградой на данном шаге и наградой, которую получили агенты за выбор маршрута).
* отобразить все потоки, которые передаются по сети в данный момент.
* визуализировать граф и загрузку очередей на каждом выходном порту.
* отобразить маршрут движения определенного потока.
* построить график недополученной награды на валидационной выборке потоков.
* в конце данного раздела также представлен пример использования средства визуализации.
