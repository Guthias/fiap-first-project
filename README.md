# 🚀 Simulação do Foguete AURORA

![](https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExcmZwcjR2aWdobjZiMDU3bjBqZjFiMjNjdTJyNGZsMGVvYzBpZ2R5MCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/dfzbVh4XaYCAalgUG8/giphy.gif)

Projeto acadêmico desenvolvido em Python para simular a telemetria de um foguete durante uma missão experimental com destino a Marte.

O projeto gera dados de telemetria artificialmente, verifica as condições iniciais para autorizar ou abortar o lançamento e, caso o lançamento seja autorizado, executa uma simulação de **600 segundos (10 minutos)**.

Durante a simulação, parâmetros como temperatura, pressão, carga da bateria e potência são atualizados e armazenados em um histórico. Ao final, os dados são organizados utilizando Pandas, apresentados em gráficos e enviados para uma **inteligência artificial**, que realiza uma análise final da telemetria.

> **Aviso:** todos os valores, faixas de operação e comportamentos utilizados no projeto são hipotéticos e foram definidos exclusivamente para fins acadêmicos. O projeto não representa um modelo físico ou operacional de um foguete real.

---

## 📋 Sobre o projeto

A simulação foi desenvolvida para representar, de forma simplificada, um sistema de monitoramento de telemetria.

O projeto é dividido nas seguintes etapas:

1. **Geração da telemetria inicial**
2. **Verificação das condições para o lançamento**
3. **Simulação de 600 segundos**
4. **Cálculo do consumo energético**
5. **Registro do histórico da telemetria**
6. **Organização dos dados com Pandas**
7. **Visualização dos dados**
8. **Análise final utilizando inteligência artificial**

---

## 📡 Parâmetros monitorados

A telemetria considera os seguintes parâmetros:

| Parâmetro | Faixa segura | Faixa utilizada na geração |
|---|---:|---:|
| Temperatura interna | 15–35 °C | 10–40 °C |
| Temperatura externa | 0–40 °C | -5–45 °C |
| Carga da bateria | ≥ 80% | 70–100% |
| Pressão do tanque | 90–110% | 85–115% |

Também são monitorados três módulos críticos:

- 💻 Computador
- 📡 Comunicação
- 🧭 Navegação

Cada módulo possui 97% de probabilidade de funcionamento e 3% de probabilidade de falha na geração da telemetria inicial.

---

## 🛫 Verificação do lançamento

Antes de iniciar a simulação, os dados iniciais são avaliados por um algoritmo determinístico.

O lançamento é autorizado somente quando:

- A temperatura interna está entre 15 °C e 35 °C;
- A temperatura externa está entre 0 °C e 40 °C;
- A bateria possui pelo menos 80% de carga;
- A pressão está entre 90% e 110%;
- Todos os módulos críticos estão funcionando.

Caso qualquer uma dessas condições não seja atendida, o lançamento é abortado e as etapas posteriores da simulação não devem ser executadas.

---

## ⏱️ Simulação

Quando o lançamento é autorizado, o programa executa uma simulação de **600 segundos**.

A cada segundo, o estado atual da telemetria é utilizado para calcular o próximo estado.

As temperaturas e a pressão recebem pequenas variações aleatórias para representar oscilações nas leituras dos sensores.

A potência é controlada de acordo com a etapa da simulação:

| Etapa | Potência |
|---|---:|
| Inicial | 100% |
| Intermediária | 70% |
| Final | 40% |

A redução da potência é planejada e faz parte do funcionamento normal da simulação.

---

## ⚡ Consumo energético

O consumo energético varia de acordo com a potência utilizada.

Para representar perdas durante o processo, é utilizada uma perda energética aleatória entre 10% e 30%, com maior concentração de valores próximos de 10%.

O consumo máximo definido para 100% de potência é de aproximadamente **0,05% da bateria por segundo**.

A bateria é atualizada a cada segundo, sendo descontado o consumo calculado para aquele instante.

---

## 📊 Organização e visualização dos dados

Os dados coletados durante a simulação são armazenados em um histórico e posteriormente convertidos em um `DataFrame` utilizando a biblioteca **Pandas**.

O dataset final possui informações sobre:

- Segundo da simulação;
- Temperatura interna;
- Temperatura externa;
- Carga da bateria;
- Pressão;
- Potência;
- Integridade dos módulos.

A integridade dos módulos é consolidada em uma única informação:

- `OK` — todos os módulos estão funcionando;
- `FALHA` — pelo menos um módulo apresenta falha.

O projeto também apresenta três gráficos:

### 🌡️ Temperatura

Apresenta as temperaturas interna e externa durante os 600 segundos.

### 🛢️ Pressão

Apresenta a evolução da pressão do tanque durante a simulação.

### ⚡ Potência e bateria

Apresenta simultaneamente a potência utilizada e a redução da carga da bateria durante a missão.

---

## 🤖 Análise utilizando inteligência artificial

A última etapa do projeto utiliza inteligência artificial para analisar os dados produzidos pela simulação.

O `DataFrame` completo é enviado para o modelo de IA juntamente com as regras e os comportamentos esperados da simulação.

A IA deve diferenciar comportamentos normais de possíveis anomalias.

Por exemplo:

- A redução de potência de 100% para 70% e posteriormente para 40% é esperada;
- A redução progressiva da bateria é esperada;
- Pequenas oscilações de temperatura e pressão são esperadas;
- Maior consumo energético durante períodos de maior potência é esperado.

Esses comportamentos não devem ser classificados como anomalias simplesmente por ocorrerem.

A IA procura principalmente desvios como:

- Temperaturas fora das faixas definidas;
- Pressão fora da faixa definida;
- Falhas nos módulos críticos;
- Oscilações anormalmente grandes;
- Mudanças abruptas não planejadas;
- Consumo incompatível com a potência utilizada;
- Combinações de eventos que possam indicar uma situação de risco.

### Resposta estruturada

Para que o resultado da IA possa ser utilizado diretamente pelo programa, a resposta não é recebida como texto livre.

A IA retorna um objeto JSON com três campos:

```json
{
    "classificacao": "RISCO BAIXO",
    "anomalias": [],
    "sugestao": "Os parâmetros permaneceram dentro das condições esperadas durante a simulação."
}
```

Os campos representam:

- `classificacao` — classificação geral do risco;
- `anomalias` — lista das anomalias identificadas;
- `sugestao` — recomendação gerada a partir dos resultados.

O Python converte a resposta utilizando `json.loads()` e utiliza essas informações para construir uma apresentação visual no Google Colab.

---

## 🧰 Tecnologias utilizadas

- **Python**
- **Google Colab**
- **Pandas** — organização e manipulação dos dados;
- **Matplotlib** — geração dos gráficos;
- **Google Gemini API** — análise da telemetria utilizando inteligência artificial;
- **IPython HTML** — apresentação visual do resultado final.
---

# ▶️ Como executar

## 1. Abrir o projeto

O projeto foi desenvolvido para ser executado no **Google Colab**.

Abra o arquivo `.ipynb` no Google Colab.

---

## 2. Configurar a API da IA

A etapa final utiliza a API do Google Gemini.

É necessário possuir uma chave de API e disponibilizá-la no Google Colab como um segredo chamado:

```text
GEMINI_API_KEY
```

A chave não deve ser inserida diretamente no código ou publicada no repositório.

O notebook recupera a chave utilizando o sistema de Secrets do Google Colab.

---

## 3. Executar o notebook

Após configurar a chave da API:

1. Abra o notebook no Google Colab;
2. Verifique se o segredo `GEMINI_API_KEY` está configurado;
3. Execute as células na ordem apresentada;
4. A telemetria inicial será gerada;
5. O lançamento será verificado;
6. Caso autorizado, a simulação de 600 segundos será executada;
7. Os dados serão organizados em um DataFrame;
8. Os gráficos de telemetria serão apresentados;
9. A análise final será enviada para a inteligência artificial;
10. O resultado da IA será apresentado no painel final.

### ⚠️ Importante

Como os dados são gerados aleatoriamente, cada execução pode produzir resultados diferentes.

Também é possível que uma execução gere condições iniciais que façam o lançamento ser abortado. Nesse caso, a simulação não deve prosseguir para as etapas que dependem de um lançamento autorizado.

---

# 🖼️ Prints da execução

Abaixo estão os espaços destinados aos registros da execução do projeto.

## 1. Telemetria inicial

![](https://github.com/Guthias/fiap-first-project/blob/main/images/telemetria-inicial.png?raw=true)

---

## 2. Verificação do lançamento

### Sucesso no Lançamento
![](https://github.com/Guthias/fiap-first-project/blob/main/images/sucesso-lancamento.png?raw=true)

### Falha no Lançamento

![](https://github.com/Guthias/fiap-first-project/blob/main/images/falha-lancamento.png?raw=true)

---

## 3. Dataset da telemetria

![](https://github.com/Guthias/fiap-first-project/blob/main/images/dataset-telemetria-completa.png?raw=true)

---

## 4. Gráfico de temperatura

![](https://github.com/Guthias/fiap-first-project/blob/main/images/grafico-temperatura.png?raw=true)

---

## 5. Gráfico de pressão

![](https://github.com/Guthias/fiap-first-project/blob/main/images/grafico-pressao.png?raw=true)
---

## 6. Gráfico de potência e bateria

![](https://github.com/Guthias/fiap-first-project/blob/main/images/grafico-bateria.png?raw=true)
---

## 7. Análise final da inteligência artificial

### Analise de Baixo Risco
![](https://github.com/Guthias/fiap-first-project/blob/main/images/analise-baixo-risco.png?raw=true)

### Analise de Médio Risco
![](https://github.com/Guthias/fiap-first-project/blob/main/images/analise-medio-risco.png?raw=true)

---

# 📁 Estrutura do projeto

```text
.
├── /images
├── notebook.ipynb
└── README.md
```

O arquivo `notebook.ipynb` contém toda a implementação da simulação, incluindo geração da telemetria, validação do lançamento, simulação, análise dos dados, visualizações e integração com a inteligência artificial.

E a pasta `/images` contém as imagens que estão sendo utilizadas nesse README

---

# 👨‍💻 Objetivo acadêmico

O projeto busca demonstrar a aplicação integrada de conceitos de programação, geração e manipulação de dados, tomada de decisão baseada em regras, visualização de informações e utilização de inteligência artificial para análise de dados.

A proposta é representar um fluxo completo no qual dados são **gerados, validados, processados, visualizados e posteriormente interpretados por uma IA**.