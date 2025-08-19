# Comparativo de Folha de Pagamento

## Sobre o Programa
Este projeto tem como objetivo extrair, consolidar e comparar dados de folhas de pagamento a partir de diferentes formatos de arquivos (.html, .xlsx, .xls e .csv).
Ao final, gera um arquivo Excel consolidado (comparativo_folha.xlsx) com as informações por matrícula, nome, valores líquidos e diferenças entre competências.

## Funcionalidades

1. Leitura de múltiplos formatos:

.html e .htm → extração usando BeautifulSoup.

.xlsx e .xls → leitura via pandas.

.csv → leitura automática com detecção de encoding via chardet.

2. Limpeza e padronização de dados:

2.1. Padroniza nomes em maiúsculo.

2.2. Converte valores líquidos para float.

2.3. Identifica início e fim do nome do servidor a partir de palavras-chave (como SUPERVISOR, GERENTE, TÉCNICO, etc.).

3. Comparação entre competências (meses/anos):

3.1. Mostra diferenças salariais.

3.2. Informa se um servidor é novo na folha, se foi retirado da folha ou se teve alteração salarial.

4. Exportação final:

4.1. Gera um arquivo Excel comparativo_folha.xlsx com os dados organizados.

## Instalação

No Google Colab (ou em um ambiente local com Jupyter), instale as dependências:
 ```sh
!pip install pandas openpyxl beautifulsoup4 chardet
```

## Estrutura de Entrada

Os arquivos devem ser carregados no ambiente via files.upload() (Colab) ou manualmente no diretório do script.

Nome do arquivo deve seguir o padrão:
```sh
<competencia>_qualquer_nome.html
<competencia>_qualquer_nome.xlsx
<competencia>_qualquer_nome.csv
```

Exemplo:
```sh
202401_folha_servidores.html
202402_folha_servidores.xlsx
```

## Onde:

competencia = referência (mês/ano) usada para comparar valores.

## Como Usar

1. Execute o código.

2. Quando solicitado, faça upload dos arquivos da folha de pagamento (HTML, Excel ou CSV).

3. O script irá:
Extrair os dados de matrícula, nome, líquido e competência; 
Consolidar todos os arquivos em um único DataFrame;
Gerar colunas de comparação mês a mês.

4. O arquivo final comparativo_folha.xlsx será baixado automaticamente.

## Estrutura do Arquivo Final

O Excel gerado (comparativo_folha.xlsx) contém:

`matricula` → número da matrícula do servidor.

`nome` → nome padronizado em maiúsculo.

`competências`  (colunas dinâmicas, ex: 202401, 202402) → valor líquido por mês.

`diferenças` (ex: 202401_para_202402) → variação do salário líquido.

`status` (ex: status_202401_para_202402) → indica se houve: Novo na folha atual; Retirado da folha atual; Há Alteração Salarial; vazio (sem alteração).


## Exemplo de Saída
| matricula | nome        | 202401  | 202402  | 202401\_para\_202402 | status\_202401\_para\_202402 |
| --------- | ----------- | ------- | ------- | -------------------- | ---------------------------- |
| 12345     | JOÃO SILVA  | 5000.00 | 5100.00 | 100.00               | Há Alteração Salarial        |
| 67890     | MARIA SOUZA | NaN     | 4000.00 | NaN                  | Novo na folha atual          |
| 11223     | ANA PEREIRA | 3500.00 | NaN     | NaN                  | Retirado da folha atual      |
