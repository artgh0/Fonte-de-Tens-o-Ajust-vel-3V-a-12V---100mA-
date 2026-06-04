## INTRODUÇÃO

Este projeto detalha o desenvolvimento de uma fonte de alimentação linear ajustável, projetada para converter a tensão alternada da rede elétrica em tensão contínua estável. O circuito contém uma topologia clássica de regulação série com transistor e diodo Zener, sendo capaz de fornecer até 100 mA.



## OBJETIVO

O objetivo desta atividade é projetar e montar uma fonte de 100 mA, que correspondam as seguintes características:

- **Tensão de entrada** de 127 V ou 220 V (rede CA).
- **Tensão de saída** ajustável entre 3 V e 12 V.
- **Corrente máxima** 100 mA.
- Manter a tensão de saída constante mesmo com pequenas variações na entrada.



## METODOLOGIA

O projeto foi dividido em quatro partes:

- Redução da tensão de rede (220 V / 127 V) para 18 V CA via transformador externo.
- Conversão em onda completa por meio de uma ponte montada manualmente com quatro diodos 1N4007.
- Suavização da tensão pulsante através de um capacitor eletrolítico de 1000 µF.
- Estabilização da saída pelo transistor BC337, tendo como referência um diodo Zener de 13 V e um potenciômetro para controle da faixa de saída.



## COMPONENTES

| Componente | Função Técnica | Resumo |
| :--- | :--- | :--- |
| **Transformador** | Redução de tensão | Reduz a tensão CA da rede para um nível seguro e adequado ao restante do circuito. |
| **Diodos (Ponte)** | Retificação | Quatro diodos em ponte retificadora permitem alimentação independente da polaridade do ciclo CA. |
| **Capacitor** | Filtragem | Armazena e libera carga para suavizar o *ripple*, mantendo o fluxo de tensão mais constante. |
| **LED** | Sinalização | Indica visualmente a presença de corrente no circuito|
| **Diodo Zener** | Referência de tensão | Limita a tensão máxima em 13 V. |
| **Potenciômetro** | Controle de saída | Resistência variável permitindo ajustar a tensão de saída entre 3 V e 12 V. |
| **Resistores** | Limitação de corrente | Protegem componentes como o LED e o Zener contra o excesso de corrente. |
| **Transistor** | Regulação de potência | Regula a corrente entregue à carga com base na tensão de referência do Zener, garante saída estável de até 100 mA. |

---

### Lista de Materiais e Orçamento 

| Componente | Função | Biblioteca EAGLE | Componente Utilizado | Preço Unitário |
| :--- | :--- | :--- | :--- | :--- |
| **Transformador** | Redução CA | (Externo à placa) | Trafo Bivolt 18+18 V / 200 mA | R$ 27,99 |
| **Diodos (x4)** | Retificação | `diode` / `1N4007` | Diodo 1N4007 | R$ 0,13 |
| **Capacitor** | Filtragem | `resistor` / `CPOL-EUE5-10.5` | 1000 µF / 50 V | R$ 1,20 |
| **Diodo Zener** | Referência | `diode` / `1N4728` | 1N4743 (13 V / 1 W) | R$ 0,22 |
| **Transistor** | Regulação | `transistor` / `BC337` | NPN BC337 (500 mA) | R$ 0,32 |
| **Potenciômetro** | Ajuste | `pot` / `3RP/1610N` | 10 kΩ Linear | R$ 1,92 |
| **Resistor (LED)** | Proteção do LED | `rcl` / `R-EU_0204/7` | 1 kΩ 5% (1 W) | R$ 0,24 |
| **Resistor (Base)** | Estabilização | `rcl` / `R-EU_0204/7` | 1,2 kΩ 5% (1/2 W) | R$ 0,08 |
| **LED** | Sinalização | `led` / `LED5MM` | LED Difuso 5 mm | R$ 0,24 |
| **Header** | Conexão | `pinhead` / `PINHD-1X2` | Pente de pinos 1×2 | R$ 0,10 |

## CIRCUITO FALSTAD

<img width="1548" height="469" alt="Screenshot from 2026-05-02 13-16-58" src="https://github.com/user-attachments/assets/ec68f788-ed1c-40da-a45c-3b9e6e13a2d1" />

[Acesse aqui](https://is.gd/RsRIYW)



## RELATÓRIO


O circuito é dividido em quatro blocos funcionais: transformação, retificação, filtragem e regulação. Cada bloco foi dimensionado com componentes que atendem aos requisitos da disciplina SSC0180. A seguir, cada etapa do projeto é explicada e calculada individualmente.


### Transformador

O primeiro passo da fonte é reduzir a tensão da rede elétrica (127 V ou 220 V) para um nível seguro e adequado ao restante do circuito. Utilizei um transformador bivolt com saída de **18 V CA**, que após a retificação e filtragem fornece a tensão contínua necessária para alimentar os demais estágios.

A escolha de 18 V CA permite garantir que, mesmo após as quedas de tensão nos diodos da ponte retificadora (~1,4 V no total) e o ripple do capacitor, ainda reste tensão suficiente para que o transistor regule a saída até 12 V com folga adequada.

**Tensão de pico após o transformador:**

$$V_{pico} = 18 \times \sqrt{2} \approx 25{,}4\text{ V}$$

**Tensão CC após a ponte retificadora (queda de 0,7 V em cada diodo, dois em condução simultânea):**

$$V_{CC} = 25{,}4 - 2 \times 0{,}7 = 24\text{ V}$$


---

###  Ponte Retificadora — 1N4007 × 4

A corrente alternada proveniente do transformador precisa ser convertida em corrente contínua para alimentar os estágios seguintes. Para isso, utilizei uma **ponte de Graetz** composta por quatro diodos 1N4007 dispostos em configuração de onda completa, que aproveita os dois semiciclos da CA.

O diodo 1N4007 foi escolhido por suportar até **1 A de corrente** e **1000 V de tensão reversa**, oferecendo uma margem de segurança para os 100 mA exigidos. Cada diodo apresenta uma queda de tensão direta de aproximadamente 0,7 V, resultando em queda total de 1,4 V na ponte (dois diodos conduzem simultaneamente).


---

### Capacitor de Filtro — 1000 µF / 50 V

Após a retificação, a tensão ainda apresenta uma oscilação chamada **ripple**, pois segue o formato pulsante da onda retificada. O capacitor eletrolítico em paralelo com a carga armazena carga elétrica e a libera nos vales da tensão, suavizando essa oscilação e mantendo a tensão mais estável.

Escolhi **1000 µF** para garantir um ripple baixo mesmo na corrente máxima de 100 mA. O valor de **50 V** de tensão suportada oferece o dobro de margem em relação ao pico de tensão do circuito (~25 V), garantindo segurança contra picos transitórios da rede.

**Cálculo do ripple para corrente máxima de 100 mA e frequência de 60 Hz:**

$$\Delta V = \frac{I_{carga}}{2 \times f \times C} = \frac{0{,}1}{2 \times 60 \times 0{,}001} \approx 0{,}83\text{ V}$$

**Tensão mínima disponível após o ripple:**

$$V_{min} = 24 - 0{,}83 \approx 23{,}2\text{ V}$$


> Com 1000 µF, o ripple de ~0,83 V é pequeno o suficiente para não comprometer a regulação do transistor.

---

###  Resistor do Zener — 2,2 kΩ

O diodo Zener precisa de um resistor em série para **limitar a corrente** que passa por ele. Sem o resistor, o Zener ficaria ligado quase diretamente entre VCC e GND, e a corrente seria limitada apenas pela resistência interna do circuito, queimando o componente.

O resistor de **2,2 kΩ** foi escolhido por ser o valor mais próximo do ideal calculado (~3 kΩ). Ele limita a corrente do Zener a um valor seguro, mantendo a tensão de referência estável em 13 V sem dissipar potência excessiva.

**Corrente pelo Zener:**

$$I_Z = \frac{V_{CC} - V_Z}{R} = \frac{24 - 13}{2200} \approx 5\text{ mA}$$

**Potência dissipada no resistor:**

$$P_R = I_Z^2 \times R = (0{,}005)^2 \times 2200 \approx 55\text{ mW}$$

**Potência dissipada no Zener:**

$$P_Z = V_Z \times I_Z = 13 \times 0{,}005 = 65\text{ mW}$$


---

### Diodo Zener — 1N4743 (13 V / 1 W)

O Zener define a **tensão de referência** do circuito. Ele conduz em modo reverso sempre que a tensão em seus terminais atinge sua tensão de ruptura (13 V), mantendo esse valor constante independentemente de variações na entrada.

Escolhi o **1N4743 de 13 V** porque, ao descontar a queda $V_{BE}$ de 0,7 V do transistor BC337, a tensão máxima de saída fica em aproximadamente **12,3 V**, superando a meta de 12 V do projeto. Zeners com tensão menor não atingiriam os 12 V de saída, tensões maiores elevariam demais a dissipação no transistor.


---

### Divisor de Base — Pot 10 kΩ + R 1,2 kΩ

O potenciômetro e o resistor de 1,2 kΩ formam um **divisor de tensão** que controla a tensão aplicada à base do transistor BC337. Variando o potenciômetro, ajusta a tensão na base e a tensão de saída da fonte.

O resistor de **1,2 kΩ** em série limita a corrente máxima de base (protegendo o BC337 caso o potenciômetro seja girado ao extremo) e garante que o transistor opere na **região ativa linear**, essencial para a regulação estável da tensão.

**Tensão mínima na base** (potenciômetro no máximo = 10 kΩ):

$$V_{B_{min}} = 13 \times \frac{1200}{1200 + 10000} \approx 1{,}4\text{ V}$$

**Tensão mínima de saída** (descontando $V_{BE} = 0{,}7$ V):

$$V_{out_{min}} = 1{,}4 - 0{,}7 \approx 0{,}7\text{ V}$$

> O transistor começa a conduzir adequadamente a partir de ~3 V na saída.

**Tensão máxima na base** (potenciômetro no mínimo = 50 Ω, limitação do simulador):

$$V_{B_{max}} = 13 \times \frac{1200}{1200 + 50} \approx 12{,}5\text{ V}$$

**Tensão máxima de saída:**

$$V_{out_{max}} = 12{,}5 - 0{,}7 \approx 11{,}8\text{ V} \approx 12\text{ V}$$



---

### Transistor BC337

O BC337 opera como **seguidor de emissor** (emitter follower), ele entrega à carga a tensão imposta pelo divisor de base, menos a queda $V_{BE}$, com capacidade de fornecer até 100 mA sem que o circuito de referência (Zener + potenciômetro) precise fornecer essa corrente diretamente.

A escolha do **BC337** se justifica por seu limite de 500 mA de corrente de coletor, bem maior do que a necessária para este projeto. O único ponto de atenção é a potência dissipada em condições extremas, pois fontes lineares convertem a diferença entre entrada e saída em calor no transistor.

**Corrente máxima de base:**

$$I_B = \frac{V_{B_{max}} - V_{BE}}{R_{base}} = \frac{12{,}5 - 0{,}7}{1200} \approx 9{,}8\text{ mA}$$

**Corrente máxima de coletor** com $h_{FE} = 100$:

$$I_C = h_{FE} \times I_B = 100 \times 9{,}8 = 980\text{ mA}$$

> A corrente é limitada pela carga a **100 mA**, bem dentro do limite do BC337.

**Potência dissipada no pior caso** (saída em 3 V, carga de 100 mA):

$$P_{Q1} = (V_{CC} - V_{out}) \times I_C = (24 - 3) \times 0{,}1 = 2{,}1\text{ W}$$


> A potência de 2,1 W supera o limite contínuo do BC337 (625 mW). Para testes rápidos, o componente opera sem risco imediato. Para uso prolongado (30+ min), recomenda-se dissipador térmico.

---

### Resistor do LED — 1 kΩ / 1 W

O LED serve como **indicador visual** de que a fonte está energizada. Como qualquer LED, ele precisa de um resistor limitador de corrente para não queimar, pois sua resistência interna é muito baixa e sem limitação a corrente seria destrutiva.

O resistor de **1 kΩ** foi escolhido para manter a corrente do LED dentro da faixa segura (~20 mA), garantindo brilho adequado sem risco de dano. A especificação de **1 W** garante folga térmica confortável, atendendo ao requisito mínimo de 1/2 W da disciplina.

**Corrente pelo LED** (queda de tensão no LED: ~2 V):

$$I_{LED} = \frac{V_{CC} - V_{LED}}{R} = \frac{24 - 2}{1000} = 22\text{ mA}$$

**Potência dissipada no resistor:**

$$P_R = I^2 \times R = (0{,}022)^2 \times 1000 \approx 484\text{ mW}$$


---

### Carga de Teste — R 1,2 kΩ

Para validar o funcionamento da fonte, utilizamos um resistor de **1,2 kΩ** como carga de teste. Ele simula um dispositivo real conectado à saída e permite verificar se a tensão se mantém estável e ajustável conforme o potenciômetro é variado.

**Corrente máxima na carga** (saída em 12 V):

$$I_{carga} = \frac{V_{out}}{R} = \frac{12}{1200} = 10\text{ mA}$$

**Potência dissipada na carga:**

$$P = \frac{V^2}{R} = \frac{144}{1200} = 120\text{ mW}$$


---

## Resumo

| Parâmetro | Valor calculado | Limite do componente | Status |
| :--- | :---: | :---: | :---: |
| Tensão CC após ponte | ~24 V | 50 V (cap) | ✅ |
| Ripple máximo (100 mA) | ~0,83 V | — | ✅ |
| Corrente pelo Zener | ~5 mA | 76 mA | ✅ |
| Potência no R 2,2kΩ | ~55 mW | 250 mW | ✅ |
| Potência no Zener | ~65 mW | 1000 mW | ✅ |
| Tensão de saída mínima | ~3 V | — | ✅ |
| Tensão de saída máxima | ~12 V | — | ✅ |
| Corrente no LED | ~22 mA | 30 mA | ✅ |
| Potência no R LED | ~484 mW | 1000 mW | ✅ |
| Corrente BC337 (prática) | 100 mA | 500 mA | ✅ |
| Potência BC337 (pior caso) | 2,1 W | 625 mW |  dissipador p/ uso longo |
| Potência na carga | 120 mW | 500 mW | ✅ |


















## CONCLUSÃO

A fonte projetada para a disciplina SSC0180 funcionou sem problemas com as especificações superiores às mínimas exigidas. O transistor BC337 em conjunto com a ponte retificadora de diodos 1N4007 garante operação estável a 100 mA, tornando a fonte uma ferramenta confiável para uso nos laboratórios do ICMC-USP.
