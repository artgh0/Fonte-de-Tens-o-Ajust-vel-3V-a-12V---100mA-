## INTRODUÇÃO

Este projeto detalha o desenvolvimento de uma fonte de alimentação linear ajustável, projetada para converter a tensão alternada da rede elétrica em tensão contínua estável. O circuito emprega uma topologia clássica de regulação série com transistor e diodo Zener, sendo capaz de fornecer até 100 mA — ideal para experimentos laboratoriais e alimentação de circuitos de pequena escala.



## OBJETIVO

Projetar e montar uma fonte de 100 mA utilizando componentes do inventário disponível, respeitando as normas de potência e as bibliotecas do EAGLE estipuladas em aula.

- **Tensão de entrada:** 127 V ou 220 V (rede CA).
- **Tensão de saída:** Ajustável entre 3 V e 12 V.
- **Corrente máxima:** 100 mA.
- **Estabilidade:** Manter a tensão de saída constante mesmo diante de pequenas variações na entrada.



## METODOLOGIA

O projeto foi dividido em quatro blocos funcionais:

- **Transformação:** Redução da tensão de rede (220 V / 127 V) para 18 V CA via transformador externo.
- **Retificação:** Conversão em onda completa por meio de uma ponte montada manualmente com quatro diodos 1N4007.
- **Filtragem:** Suavização da tensão pulsante através de um capacitor eletrolítico de 1000 µF.
- **Regulação e ajuste:** Estabilização da saída pelo transistor BC337, tendo como referência um diodo Zener de 13 V e um potenciômetro para controle da faixa de saída.



## COMPONENTES

Esta seção detalha o papel de cada componente no sistema, fundamentado nos conceitos da disciplina SSC0180 — Eletrônica para Computação.

### 🛠️ Detalhamento dos Componentes

| Componente | Função Técnica | Explicação Detalhada |
| :--- | :--- | :--- |
| **Transformador** | Redução de tensão | Reduz a tensão CA da rede para um nível seguro e adequado ao restante do circuito. |
| **Diodos (Ponte)** | Retificação | Quatro diodos em ponte retificadora permitem alimentação independente da polaridade do ciclo CA. |
| **Capacitor** | Filtragem | Armazena e libera carga para suavizar o *ripple*, mantendo o fluxo de tensão mais constante. |
| **LED** | Sinalização | Indica visualmente a presença de corrente no circuito; não atua na regulação de tensão. |
| **Diodo Zener** | Referência de tensão | Limita a tensão máxima em 13 V, servindo de referência estável para a base do transistor. |
| **Potenciômetro** | Controle de saída | Resistência variável que permite ao usuário ajustar a tensão de saída entre 3 V e 12 V. |
| **Resistores** | Limitação de corrente | Protegem componentes como o LED e o Zener contra fluxo excessivo de corrente. |
| **Transistor** | Regulação de potência | Regula a corrente entregue à carga com base na tensão de referência do Zener, garantindo saída estável de até 100 mA. |

### 📋 Lista de Materiais e Orçamento 

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

---

### ⚙️ Notas de Projeto

- **Faixa de tensão:** O circuito opera entre **2,8 V e 12,3 V**. O Zener de 13 V compensa a queda $V_{BE}$ do transistor BC337.
- **Potência térmica:** O resistor do LED é de **1 W**, superando o mínimo de 1/2 W exigido pela disciplina.
- **Segurança:** O capacitor de **50 V** garante operação segura contra picos da tensão retificada (~25 V de pico).
- **Pinagem BC337:** Atenção ao layout — 1: Coletor, 2: Base, 3: Emissor.



## RELATÓRIO

[Acesse aqui](https://is.gd/NnS61O)

> Como a fonte é do tipo linear, a diferença entre a tensão de entrada e a de saída é dissipada em calor, principalmente no transistor. Em uma saída de 3 V com carga máxima (100 mA), o componente precisa dissipar o excesso de energia, tornando o uso de dissipador térmico recomendado para preservar a vida útil dos semicondutores.

### Margem de Erro e Tolerância

- **Compensação de queda:** O BC337 consome aproximadamente 0,7 V entre Base e Emissor ($V_{BE}$). Com o Zener de 13 V, a saída máxima atinge 12,3 V, garantindo a meta de 12 V mesmo com as perdas internas.
- **Faixa inferior:** A tensão mínima de 2,8 V permite alimentação segura de circuitos lógicos de 3,3 V.
- **Sobredimensionamento:** O capacitor de 50 V suporta o dobro da tensão de pico retificada (~25 V), e o resistor do LED de 1 W opera com ampla folga térmica em relação ao mínimo exigido de 1/2 W.



## CONCLUSÃO

A fonte projetada para a disciplina SSC0180 demonstra robustez ao utilizar componentes com especificações superiores às mínimas exigidas — resistores de 1 W e capacitor de 50 V. O transistor BC337 em conjunto com a ponte retificadora de diodos 1N4007 garante operação estável a 100 mA, tornando a fonte uma ferramenta confiável para uso nos laboratórios do ICMC-USP.
