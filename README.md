# address-validator-gemini
Processador inteligente de endereços em Google Colab: lê de planilhas, consulta CEPs, valida/corrige com IA (Gemini), geolocaliza e atualiza os dados.


# Validador e Enriquecedor Inteligente de Endereços com IA

**Versão:** 1.0.0
**Data da Última Atualização:** 17 de maio de 2025
**Autor(es):** [Seu Nome/Nome da Equipe Aqui]

## 🎯 Introdução e Objetivo do Projeto

Em muitas operações de negócios, a qualidade dos dados de endereço é crucial. No entanto, é comum encontrar desafios significativos quando se lida com informações provenientes de sistemas legados, onde a validação de endereços na entrada pode não ter sido rigorosa.

**O Problema Real:**

Nossa equipe frequentemente presta serviços para clientes que dependem de sistemas mais antigos. Um desafio recorrente é a grande quantidade de endereços de clientes que são cadastrados sem uma validação adequada no momento da entrada. Isso resulta em uma base de dados com:

* Abreviações inconsistentes (R, RUA, AV, AVDA, etc.).
* Erros de digitação em logradouros, bairros e cidades.
* CEPs incorretos, desatualizados ou mal formatados.
* Informações faltantes, como bairro, CEP ou até mesmo a cidade correta para um dado logradouro.

Atualmente, o processo de correção desses milhares de endereços problemáticos é manual ou baseado em scripts PL/SQL reativos, que tentam ajustar os dados conforme os erros são descobertos. Essa abordagem é trabalhosa, demorada, propensa a novos erros e não consegue lidar com a complexidade e variedade dos problemas de forma proativa e inteligente.

**O Objetivo Deste Projeto:**

Este projeto visa **automatizar e aprimorar radicalmente o processo de validação, correção e enriquecimento desses endereços  problemáticos.** Utilizando uma abordagem moderna com:

1.  **Consulta de CEP via API ViaCEP:** Para obter um endereço de referência confiável a partir do CEP.
2.  **Inteligência Artificial com Google Gemini:** Para realizar uma análise avançada, comparando o endereço original com os dados do CEP, padronizando abreviações, corrigindo erros de forma contextual, preenchendo lacunas e tomando decisões inteligentes sobre o endereço mais preciso.
3.  **Geolocalização com Geopy:** Para obter as coordenadas geográficas (latitude e longitude) do endereço final corrigido.
4.  **Integração com Google Sheets:** Para ler os endereços problemáticos e atualizar a planilha com os dados validados, corrigidos, enriquecidos e georreferenciados, incluindo um link para o Google Maps.

O sistema é implementado como um notebook Google Colab para facilitar a execução, o compartilhamento e a adaptação, buscando reduzir significativamente o esforço manual, aumentar a precisão dos dados de endereço e fornecer informações mais ricas para tomada de decisão.
## ✨ Funcionalidades Principais

* **Leitura de Endereços:** Lê dados de endereço diretamente de uma Planilha Google especificada.
* **Consulta de CEP:** Utiliza a API ViaCEP para buscar informações de logradouro, bairro, cidade e UF a partir do CEP fornecido na planilha.
* **Validação e Correção com IA (Google Gemini):**
    * Padroniza abreviações de tipos de logradouro (ex: R. para Rua, Av. para Avenida).
    * Compara o endereço original da planilha com os dados obtidos pela consulta do CEP.
    * Utiliza inteligência artificial para analisar divergências, corrigir erros de digitação, preencher informações faltantes e tomar decisões sobre o endereço mais provável e correto.
    * Avalia o endereço como um todo para garantir coerência.
* **Formatação de Saída:**
    * Gera um "Endereço Completo Final" formatado em uma única coluna.
    * Fornece um "Status Final da Validação" claro e informativo.
    * Mantém campos corrigidos individualmente (logradouro, número, CEP, etc.).
* **Geolocalização:** Obtém as coordenadas de latitude e longitude do endereço final corrigido.
* **Link para Google Maps:** Gera um link direto para o Google Maps com base no endereço ou nas coordenadas.
* **Atualização da Planilha:** Escreve todos os resultados (dados do ViaCEP, endereço corrigido, status, observações da IA, geolocalização, link do mapa) de volta na Planilha Google original em novas colunas.

## ⚙️ Fluxo de Dados Simplificado

1.  **Entrada:** O script lê uma lista de endereços de uma aba específica em uma Planilha Google. Cada linha contém campos como Logradouro Original, Número Original, CEP Original, etc.
2.  **Consulta ViaCEP:** Para cada endereço, o CEP Original (se fornecido) é usado para consultar a API ViaCEP, obtendo dados de referência (logradouro, bairro, cidade, UF do CEP).
3.  **Processamento com Gemini:** O endereço original da planilha e os dados do ViaCEP são enviados ao modelo Google Gemini. O Gemini realiza:
    * Padronização de abreviações.
    * Comparação entre o endereço da planilha e o do ViaCEP.
    * Correção de erros e preenchimento de lacunas com base na análise.
    * Geração de um endereço completo formatado e um status de validação.
4.  **Geolocalização:** O endereço final corrigido é enviado para um serviço de geocodificação (Geopy/Nominatim) para obter latitude e longitude. Um link do Google Maps é gerado.
5.  **Saída:** A Planilha Google é atualizada com todas as novas informações em colunas adicionais para cada endereço processado.

## 🛠️ Tecnologias Utilizadas

* **Python 3.x**
* **Google Colab:** Ambiente de execução do notebook.
* **Google Gemini API:** Para inteligência artificial e validação avançada de endereços.
* **Google Sheets API (via `gspread`):** Para interação com Planilhas Google.
* **ViaCEP API (via `requests`):** Para consulta de endereços por CEP.
* **Geopy (Nominatim):** Para geolocalização.
* **Pandas:** Para manipulação de dados tabulares.

## 🚀 Guia de Utilização (Execução no Google Colab)

Siga estes passos para configurar e executar o projeto:

### Parte 1: Configurações Iniciais (Feitas Apenas Uma Vez)

1.  **Obter Chave de API do Google Gemini:**
    * Acesse o [Google AI Studio](https://aistudio.google.com/app/apikey).
    * Crie ou selecione um projeto e gere uma chave de API.
    * Copie e guarde esta chave de forma segura.

2.  **Configurar Projeto no Google Cloud Platform (GCP):**
    * Acesse o [Google Cloud Console](https://console.cloud.google.com/).
    * Selecione o mesmo projeto usado para a API Gemini ou crie um novo.
    * Ative as seguintes APIs:
        * **Google Sheets API**
        * **Google Drive API**

3.  **Criar Conta de Serviço e Baixar Credenciais (`credentials.json`):**
    * No GCP, vá para "IAM e Admin" > "Contas de serviço".
    * Clique em "+ CRIAR CONTA DE SERVIÇO".
    * Dê um nome (ex: `sheet-editor-bot`) e clique em "CRIAR E CONTINUAR".
    * Conceda o papel de **"Editor"** (Projeto > Editor) para simplificar. Clique em "CONTINUAR".
    * Pule a etapa de conceder acesso a usuários e clique em "CONCLUÍDO".
    * Na lista de contas de serviço, encontre a que você criou, clique nos três pontos (⋮) > "Gerenciar chaves".
    * Clique em "ADICIONAR CHAVE" > "Criar nova chave".
    * Escolha **JSON** e clique em "CRIAR". O arquivo será baixado.
    * **Renomeie o arquivo baixado para `credentials.json`**.

4.  **Preparar a Planilha Google Sheets:**
    * Crie uma nova Planilha Google ou use uma existente.
    * **Copie o ID da Planilha** da URL (fica entre `/d/` e `/edit`).
    * Nomeie uma aba para os dados de entrada (ex: `enderecos_com_problema`).
    * Na primeira linha desta aba, crie os seguintes cabeçalhos:
        * `Logradouro Original`
        * `Número Original`
        * `Complemento Original`
        * `Bairro Original`
        * `Cidade Original`
        * `UF Original`
        * `CEP Original`
    * **Compartilhe esta Planilha** com o e-mail da conta de serviço que você criou (encontrado no `credentials.json`, campo `client_email`). Dê permissão de **"Editor"**.

### Parte 2: Configurando e Executando o Notebook Colab

1.  **Abrir/Preparar o Notebook Colab:**
    * Abra o notebook Colab contendo o projeto. Se estiver começando do zero, crie um novo notebook e copie as células de código fornecidas.

2.  **Configurar Secrets no Colab:**
    * No Colab, clique no ícone de chave (🔑) "Secrets" na barra lateral esquerda.
    * Ative "Acesso ao notebook".
    * Adicione um novo secret:
        * Nome: `GEMINI_API_KEY`
        * Valor: Cole sua chave de API do Gemini.

3.  **Fazer Upload do `credentials.json`:**
    * No Colab, clique no ícone de pasta "Arquivos" na barra lateral esquerda.
    * Clique em "Fazer upload" e selecione seu arquivo `credentials.json`.

4.  **Ajustar Configurações na Célula 2:**
    * Abra a Célula 2 (bloco de código referente ao `config.py`).
    * Substitua `"SEU_GOOGLE_SHEET_ID_AQUI"` pelo ID real da sua planilha.
    * Verifique se `SHEET_NAME_TO_PROCESS` corresponde ao nome da sua aba.
    * Confirme se os nomes das colunas (`COL_..._ORIGINAL`) estão corretos.

5.  **Executar as Células em Ordem:**
    * Execute cada célula do notebook, uma por uma, na ordem fornecida:
        1.  **Célula 1: Instalar Dependências**
        2.  **Célula 2: Configurações**
        3.  **Célula 3: Consulta de CEP (cep_handler.py)**
        4.  **Célula 4: Prompts para o Gemini (prompts.py)**
        5.  **Célula 5: Gemini Handler (gemini_handler.py)**
        6.  **Célula 6: Geolocalização (geocoding_handler.py)**
        7.  **Célula 7: Interação com Google Sheets (google_sheets_handler.py)**
        8.  **Célula 8: Script Principal (main.py)**
    * Para executar uma célula, clique no ícone de "play" (▶️) ou pressione `Shift + Enter`.

6.  **Adicionar Dados de Entrada:**
    * Antes de executar a Célula 8 (ou enquanto ela roda), preencha a aba da sua Planilha Google com os endereços que você deseja processar, seguindo os cabeçalhos definidos.

### Parte 3: Entendendo os Resultados

* Após a execução completa da Célula 8, a sua Planilha Google será atualizada.
* Novas colunas serão adicionadas (se não existiam) à direita das suas colunas de entrada, contendo:
    * Dados retornados pela consulta ViaCEP (`Logradouro (ViaCEP)`, `Status Consulta CEP`, etc.).
    * Campos de endereço final corrigidos e padronizados pelo Gemini (`Logradouro Final Corrigido`, etc.).
    * A coluna `Endereço Completo Final`.
    * O `Status Final da Validação`.
    * `Observações da Validação (Gemini)`.
    * `JSON Retorno Gemini (Final)` para auditoria.
    * `Latitude`, `Longitude`.
    * `Link Google Maps`.
* O output da Célula 8 no Colab fornecerá logs detalhados do processo, incluindo informações de cada etapa, alertas e possíveis erros.

## 🏗️ Estrutura do Código (Células do Notebook Colab)

* **Célula 1: Instalar Dependências:** Instala bibliotecas Python necessárias.
* **Célula 2: `config.py`:** Define todas as configurações globais (API Keys, IDs de Planilha, nomes de colunas, URLs de API).
* **Célula 3: `cep_handler.py`:** Contém a lógica para limpar CEPs e consultar a API ViaCEP.
* **Célula 4: `prompts.py`:** Define o prompt detalhado que instrui o modelo Gemini sobre como realizar a validação multifase.
* **Célula 5: `gemini_handler.py`:** Gerencia a interação com a API Gemini, formata o prompt e processa a resposta JSON.
* **Célula 6: `geocoding_handler.py`:** Responsável por obter coordenadas de latitude/longitude usando Geopy e gerar links do Google Maps.
* **Célula 7: `google_sheets_handler.py`:** Contém funções para ler dados da Planilha Google e atualizar a planilha com os resultados.
* **Célula 8: `main.py`:** O script principal que orquestra todo o fluxo de trabalho, chamando as funções dos outros "módulos" (células) em sequência para cada endereço.

## ⚠️ Limitações e Pontos de Atenção

* **Precisão da IA:** Embora o Gemini seja avançado, a correção de endereços muito ambíguos ou com erros grosseiros pode não ser 100% precisa. Recomenda-se revisão para casos críticos.
* **API ViaCEP:** É um serviço externo gratuito; sua disponibilidade e precisão podem variar.
* **Geocodificação (Nominatim):** Serviço gratuito do OpenStreetMap, sujeito a limites de uso e a precisão pode variar dependendo da qualidade do endereço.
* **Cotas de API:** A API do Google Gemini possui cotas de uso. O script inclui um `time.sleep(1)` básico entre as chamadas para mitigar, mas para grandes volumes, pode ser necessário um tratamento mais robusto.
* **Segurança de Credenciais:** **NUNCA** exponha sua `GEMINI_API_KEY` ou o conteúdo do `credentials.json` em código público ou repositórios Git. Utilize o sistema de "Secrets" do Colab para a API Key e adicione `credentials.json` ao seu `.gitignore` se estiver usando um repositório Git localmente antes de subir.

## 💡 Possíveis Melhorias Futuras

* Interface web simples (Streamlit, Flask) para facilitar o uso.
* Dashboard para visualização de estatísticas de validação.
* Opção de upload de arquivo CSV/Excel em vez de depender apenas do Google Sheets.
* Mecanismo de feedback para treinar/ajustar os prompts com base em correções manuais.
* Integração com bancos de dados DNE (Diretório Nacional de Endereços) dos Correios para validação oficial (requer acesso/licenciamento).
* Tratamento mais sofisticado de limites de taxa de API (ex: backoff exponencial).

Exemplo de endereços com problema: 
![image](https://github.com/user-attachments/assets/262abbec-f92e-43f1-9848-6d3a3a1611f5)¨

Endereços ajustados/corrigidos:

![image](https://github.com/user-attachments/assets/44dd8741-e5ec-478a-a297-5d076b8b327f)




