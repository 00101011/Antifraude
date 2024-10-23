# Sistema Antifraude Usando Azure AI e Machine Learning

## Introdução

Este projeto visa implementar uma solução de análise automatizada de documentos usando **Azure AI** para identificar padrões de fraude, validar a autenticidade de documentos e aumentar a segurança em transações e processos empresariais. Isso garante maior confiabilidade no processamento de documentos sensíveis.

## Tecnologias Utilizadas

- **Azure Form Recognizer**: Para extração automática de dados estruturados e não estruturados.
- **Azure Machine Learning**: Para treinamento e execução de modelos de detecção de fraude.
- **Azure Blob Storage**: Para armazenamento seguro de documentos.
- **Python SDK do Azure**: Para integração e automação das APIs e serviços Azure.

## Etapas do Desenvolvimento

### 1. Configuração do Ambiente no Azure

Primeiro, foi necessário configurar os serviços do Azure:
- **Form Recognizer**: Para extrair dados de documentos.
- **Azure Machine Learning**: Para criar e treinar modelos de detecção de fraude.
- **Azure Blob Storage**: Para armazenar os documentos analisados.
- **Azure Functions** ou **Logic Apps**: Para automação e integração.

### 2. Extração de Dados com Azure Form Recognizer

Aqui está o código para extrair dados dos documentos usando **Form Recognizer**:

```python
from azure.ai.formrecognizer import DocumentAnalysisClient
from azure.core.credentials import AzureKeyCredential

# Credenciais do Azure Form Recognizer
endpoint = "https://<seu-endpoint>.cognitiveservices.azure.com/"
key = "<sua-chave>"

# Criar o cliente
document_analysis_client = DocumentAnalysisClient(
    endpoint=endpoint, credential=AzureKeyCredential(key)
)

# Caminho do documento
document_path = "<caminho-do-documento>"

# Função para ler o documento
def analyze_document(path):
    with open(path, "rb") as document:
        poller = document_analysis_client.begin_analyze_document(
            "prebuilt-layout", document
        )
        result = poller.result()
    
    # Extrair e exibir campos
    for page in result.pages:
        print(f"Page number: {page.page_number}")
        for line in page.lines:
            print(f"Line: {line.content}")
    
    # Retornar resultado bruto para análise
    return result

# Executar análise
result = analyze_document(document_path)

```
