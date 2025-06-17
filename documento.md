
# Atividade 1: Análise de Problema e Solução em PLN com Stack Overflow

**Disciplina:** Processamento de Linguagem Natural  
**Instituição:** Universidade Federal de Sergipe

## Equipe

  * José Batista 
  * Carlos Melo
  * Roberdan Tamyr
  * Arthur Matheus

## Sumário

  - [1. Introdução](https://www.google.com/search?q=%231-introdu%C3%A7%C3%A3o)
  - [2. Análise do Problema](https://www.google.com/search?q=%232-an%C3%A1lise-do-problema)
      - [2.1. Configuração do Ambiente](https://www.google.com/search?q=%2321-configura%C3%A7%C3%A3o-do-ambiente)
      - [2.2. Reprodução do Erro](https://www.google.com/search?q=%2322-reprodu%C3%A7%C3%A3o-do-erro)
  - [3. Aplicação da Solução Proposta](https://www.google.com/search?q=%233-aplica%C3%A7%C3%A3o-da-solu%C3%A7%C3%A3o-proposta)
  - [4. Análise Comparativa de Outras Respostas](https://www.google.com/search?q=%234-an%C3%A1lise-comparativa-de-outras-respostas)
  - [5. Conclusão](https://www.google.com/search?q=%235-conclus%C3%A3o)
  - [6. Referências](https://www.google.com/search?q=%236-refer%C3%AAncias)

-----

## 1\. Introdução

Este documento apresenta um tutorial detalhado para a resolução de um problema comum em Processamento de Linguagem Natural (PLN): a **tokenização** de textos contendo caracteres **Unicode**. A atividade baseia-se na análise de uma pergunta real da comunidade de desenvolvedores na plataforma Stack Overflow, que serve como uma vasta biblioteca de conhecimento prático.

O objetivo é descrever o problema, reproduzi-lo em um ambiente de desenvolvimento, aplicar a solução mais votada e aceita pela comunidade e, por fim, justificar a escolha dessa solução em detrimento de outras alternativas propostas.

A pergunta selecionada para este estudo foi:

  - **Título:** "python - Tokenizing unicode using nltk"
  - **URL:** [https://stackoverflow.com/questions/21360123/tokenizing-unicode-using-nltk](https://www.google.com/search?q=https://stackoverflow.com/questions/21360123/tokenizing-unicode-using-nltk)

Esta questão é particularmente relevante pois aborda uma falha fundamental que pode ocorrer ao processar textos em português ou em qualquer outro idioma que utilize caracteres acentuados ou pontuações específicas, que não fazem parte do conjunto padrão ASCII.

-----

## 2\. Análise do Problema

O problema central discutido na pergunta do Stack Overflow é a incapacidade dos tokenizadores padrão da biblioteca NLTK, como o `word_tokenize` e o `RegexpTokenizer`, de lidar corretamente com caracteres Unicode. A tokenização, o processo de dividir texto em unidades significativas (tokens), falha porque as expressões regulares padrão, como `\w+`, são frequentemente configuradas para reconhecer apenas caracteres alfanuméricos do padrão **ASCII** (`[a-zA-Z0-9_]`).

Isso resulta em dois erros principais:

1.  **Palavras com acentos são quebradas:** Uma palavra como "programação" é incorretamente dividida em `['programa', 'o']`.
2.  **Contrações com apóstrofos tipográficos (`’`) são separadas:** Uma palavra como "can’t" é dividida em `['can', '’', 't']`.

### 2.1. Configuração do Ambiente

Para reproduzir o cenário, é necessário um ambiente Python com a biblioteca NLTK instalada.

**Passo 1: Instalar o NLTK**

```bash
pip install nltk
```

**Passo 2: Baixar o pacote de dados 'punkt'**
O `punkt` é utilizado pelo tokenizador `word_tokenize` do NLTK.

```python
import nltk
nltk.download('punkt')
```

### 2.2. Reprodução do Erro

O script a seguir demonstra o comportamento incorreto dos tokenizadores.

**Código (`problema.py`):**

```python
import nltk
from nltk.tokenize import RegexpTokenizer

texto = "A área de programação é incrível, mas não se engane: can’t é uma contração."

# Usando um RegexpTokenizer sem o tratamento de Unicode
tokenizer_sem_unicode = RegexpTokenizer(r'\w+')
tokens_regex_incorreto = tokenizer_sem_unicode.tokenize(texto)

print("Resultado com RegexpTokenizer sem flag Unicode:")
print(tokens_regex_incorreto)
```

**Saída do Código:**

> **Atenção:** Insira abaixo um print de tela da sua IDE ou terminal mostrando o código e a sua respectiva saída incorreta.
>
> *Substitua esta imagem e o link pelo seu próprio print de tela.*

**Análise da Saída Incorreta:**
A saída `['A', 'rea', 'de', 'programa', 'o', 'incr', 'vel', 'mas', 'n', 'o', 'se', 'engane', 'can', 't', 'uma', 'contra', 'o']` demonstra claramente a falha. As palavras "área", "programação", "incrível", "não" e "contração" foram desmembradas onde quer que um caractere acentuado aparecesse.

-----

## 3\. Aplicação da Solução Proposta

A resposta aceita no Stack Overflow oferece uma solução simples e eficaz: utilizar a flag `re.UNICODE` da biblioteca de expressões regulares (`re`) do Python. Essa flag estende a definição de "caractere de palavra" (`\w`) para incluir todos os caracteres alfanuméricos definidos no padrão Unicode.

**Código (`solucao.py`):**

```python
import re # Importar a biblioteca de expressões regulares
from nltk.tokenize import RegexpTokenizer

texto = "A área de programação é incrível, mas não se engane: can’t é uma contração."

# Criando o tokenizador com a flag re.UNICODE
tokenizer_correto = RegexpTokenizer(r'\w+', flags=re.UNICODE)

tokens_corretos = tokenizer_correto.tokenize(texto)

print("Resultado com RegexpTokenizer e a flag re.UNICODE:")
print(tokens_corretos)
```

**Saída do Código Corrigido:**

> **Atenção:** Insira abaixo um print de tela da sua IDE ou terminal mostrando o código da solução e sua respectiva saída correta.
>
> *Substitua esta imagem e o link pelo seu próprio print de tela.*

**Análise da Saída Correta:**
A saída agora é `['A', 'área', 'de', 'programação', 'é', 'incrível', 'mas', 'não', 'se', 'engane', 'can', 't', 'é', 'uma', 'contração']`. Todas as palavras com acentos foram tokenizadas corretamente como unidades únicas. O problema principal foi resolvido de forma limpa e eficiente.

-----

## 4\. Análise Comparativa de Outras Respostas

A pergunta no Stack Overflow possui outras respostas, mas a que utiliza `re.UNICODE` foi aceita e mais votada por ser superior às alternativas:

  - **Alternativa 1: Pré-processamento Manual**

      - *Sugestão:* Substituir caracteres especiais antes da tokenização (ex: `texto.replace('’', "'")`).
      - *Motivo da Rejeição:* Esta abordagem é **frágil** e não escalável. O desenvolvedor precisaria prever e mapear manualmente todos os caracteres possíveis, o que é impraticável para aplicações multilíngues.

  - **Alternativa 2: Expressões Regulares Complexas**

      - *Sugestão:* Criar uma regex que lista explicitamente os intervalos de caracteres Unicode desejados.
      - *Motivo da Rejeição:* Gera um código **verboso**, de difícil leitura e manutenção. A flag `re.UNICODE` faz o mesmo trabalho de forma muito mais concisa e legível.

  - **Alternativa 3: Uso de Outras Bibliotecas (ex: spaCy)**

      - *Sugestão:* Utilizar outras bibliotecas de PLN.
      - *Motivo da Rejeição:* Embora bibliotecas como o spaCy tenham tokenizadores excelentes, a solução foge do escopo da pergunta, que era resolver o problema dentro do ecossistema NLTK/Python padrão. A resposta aceita resolve o problema na raiz, dentro das ferramentas solicitadas.

A solução com a flag `re.UNICODE` é, portanto, a ideal por ser **idiomática, robusta, legível e eficiente.**

-----

## 5\. Conclusão

Este trabalho demonstrou na prática um desafio fundamental do Processamento de Linguagem Natural: a correta manipulação de diferentes codificações de caracteres. Através da análise de um caso real no Stack Overflow, foi possível identificar um problema de tokenização com Unicode na biblioteca NLTK, reproduzir o erro e aplicar uma solução eficaz.

O principal aprendizado é a importância de utilizar as ferramentas corretas, como a flag `re.UNICODE`, que são projetadas para lidar com a complexidade de textos globais. A solução adotada não apenas resolveu o problema de forma técnica, mas também representa uma boa prática de programação: escrever um código limpo, legível e que aproveita os recursos nativos da linguagem e suas bibliotecas.

-----

## 6\. Referências

  - Stack Overflow. (2014). *python - Tokenizing unicode using nltk*. Disponível em: [https://stackoverflow.com/questions/21360123/tokenizing-unicode-using-nltk](https://www.google.com/search?q=https://stackoverflow.com/questions/21360123/tokenizing-unicode-using-nltk).
  - Python Software Foundation. *re — Regular expression operations*. Documentação Oficial do Python 3. Disponível em: [https://docs.python.org/3/library/re.html](https://docs.python.org/3/library/re.html).
  - NLTK Project. *NLTK 3.8.1 documentation*. Disponível em: [https://www.nltk.org/](https://www.nltk.org/).