## Instalando o SDK do `Google generative AI`

O SDK Python para a API Gemini está contido no pacote [`google-generativeai`](https://pypi.org/project/google-generativeai/). Instale a dependência usando o gerenciador de pacotes `pip`0:

```python
!pip install -q -U google-generativeai
```

#### Importando o SDK

```python
import google.generativeai as genai
```

## Configurando a API key

Para utilizar a API Gemini, é necessário a utilização de uma `API Key`, que é disponibilizado pela própria Google na plataforma [Google AI Studio](https://aistudio.google.com/app/apikey)

No Colab, adicione a chave "🔑" ao gerenciador de segredos a partir do menu lateral no canto esquerdo. Dê a ela o nome `GOOGLE_API_KEY`.

```python
from google.colab import userdata

GOOGLE_API_KEY=userdata.get('GOOGLE_API_KEY')
genai.configure(api_key=GOOGLE_API_KEY)
```

#### Listar os modelos generativos disponíveis

```python
for model in genai.list_models():
  if 'generateContent' in model.supported_generation_methods:
    print(model.name)
```

#### Configurar como será a geração de conteúdo do modelo

```python
generation_config = { 
    "candidate_count" : 1,
    "temperature"     : 0.5, # controle de aleatoriedade das respostas
    "top_p"           : 1    # limita o vocabulário considerado pelo modelo 
                             # durante a geração de texto
    #"top_k"           :     # limita o vocabulário com base na probabilidade 
                             # acumulada
 }
```

#### Configurar como será a utilização de **linguagem explícita**.

Alguns conteúdos gerados podem ser ofensivos a determinados públicos alvos, essa configuração tende a filtrar respostas que contenham linguagem imprópria ou ofensiva como discurso de ódio ou discriminação, ataques pessoais ou assédio, e que contenham conteúdo sexualmente sugestivo ou explícito.

```python
safety_settings = {
    "HARASSMENT" : "BLOCK_NONE",
    "HATE"       : "BLOCK_NONE",
    "SEXUAL"     : "BLOCK_NONE",
    "DANGEROUS"  : "BLOCK_NONE"
}
```

- Inicializando o modelo

```python
model = genai.GenerativeModel(
  model_name        = "gemini-1.0-pro",
  generation_config = generation_config,
  safety_settings   = safety_settings
)
```

- Gerar conteudo

```python
response = model.generate_content("Estou aprendendo sobre IA, me de sugestões de aprendizado.")
print(response.text)
```

- Inicializando o chat

```python
chat = model.start_chat(
  history = []
)
```

```
prompt = input("Esperando prompt: ")

while prompt != "fim":
  response = chat.send_message(prompt)
  print("Resposta: ", response.text, "\n")
  prompt = input("Esperando prompt: ")
```

## Aperfeiçoando o UI

#### Instalando bibliotecas adicionais

```python
!pip install -U emojis
```

Exibindo o chat de forma mais apresentavel aos usuários.

```
import textwrap
import emojis

from IPython.display import display
from IPython.display import Markdown

def to_markdown(text):
  text = text.replace('-', '  *')
  return Markdown(textwrap.indent(text, '> ', predicate=lambda _: True))

# Imprimindo o histórico
for message in chat.history:
  if (message.role in 'model'):
    emoji = emojis.encode(':robot:')
    role  = 'Chatbot primo da Luri'

display(to_markdown(f'{emoji} **{role}**: {message.parts[0].text}'))
```

- **Resposta:**
  
🤖 Chatbot primo da Luri: A chance de a Inteligência Artificial (IA) deixar de existir em um futuro próximo é extremamente baixa. A IA é um campo em rápido crescimento, com amplas aplicações no mundo real. Espera *se que a IA continue a desempenhar um papel cada vez mais importante em nossas vidas nos próximos anos.

Existem vários fatores que tornam improvável que a IA deixe de existir:

Avanços contínuos em tecnologia: Os avanços contínuos em poder computacional, algoritmos e disponibilidade de dados estão impulsionando o progresso da IA.
Ampla gama de aplicações: A IA está sendo usada em uma ampla gama de indústrias, incluindo saúde, finanças, manufatura e transporte. Esses setores dependem cada vez mais da IA para melhorar a eficiência, a tomada de decisão e a inovação.
Investimentos significativos: Governos, empresas e instituições de pesquisa estão investindo pesadamente em pesquisa e desenvolvimento de IA. Esses investimentos garantem que o campo continue a avançar.
Apoio social: A IA tem amplo apoio social, com a maioria das pessoas reconhecendo seus benefícios potenciais. Isso cria um ambiente favorável para o crescimento e desenvolvimento contínuos da IA.
Embora seja possível que ocorram retrocessos ou desafios no campo da IA, é altamente improvável que a IA deixe de existir completamente em um futuro próximo.


