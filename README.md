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

### 3. Simulação de um Modelo de Detecção de Fraudes

Abaixo está um modelo fictício de detecção de fraudes que se baseia em características extraídas do documento:

**Função para Simular Extração de Dados**
```
import random

# Simulação da extração de dados de um documento após o uso do Form Recognizer
def extract_features_from_document(document_data):
    # Campos comuns em documentos, como valores monetários, datas, assinaturas, etc.
    extracted_data = {
        "valor_total": random.uniform(1000, 50000),  # valor do documento
        "assinatura_coerente": random.choice([True, False]),  # assinatura válida ou não
        "datas_coerentes": random.choice([True, False]),  # datas consistentes no documento
        "selos_validos": random.choice([True, False]),  # se os selos/carimbos são válidos
    }
    return extracted_data
```
**Função de Detecção de Fraude**
```
# Função que simula a predição do modelo de fraude
def fraud_detection_model(features):
    # Regras simples para determinar fraude
    if features["valor_total"] > 30000 and not features["assinatura_coerente"]:
        return True
    if not features["datas_coerentes"] or not features["selos_validos"]:
        return True
    return False
```
### 4. Validação do Documento
Abaixo está a função que combina a extração de dados com o modelo de detecção de fraude:

```
# Função que valida se o documento possui indícios de fraude
def validate_fraud(document_data):
    # Extrair os recursos/características do documento
    features = extract_features_from_document(document_data)
    
    # Simular a predição de fraude usando o modelo fictício
    is_fraud = fraud_detection_model(features)
    
    # Exibir o resultado
    if is_fraud:
        print("Possível fraude detectada!")
        print(f"Detalhes: {features}")
    else:
        print("Documento considerado autêntico.")
        print(f"Detalhes: {features}")

# Simular a análise de um documento
document_data = result  # Usaria os dados extraídos anteriormente com o Form Recognizer
validate_fraud(document_data)
```
**Saída Esperada:**
Aqui estão exemplos de saídas possíveis:

Caso de Fraude Detectada:
```
Possível fraude detectada!
Detalhes: {'valor_total': 45123.67, 'assinatura_coerente': False, 'datas_coerentes': True, 'selos_validos': False}
Caso de Documento Autêntico:
```

Documento considerado autêntico.
```
Detalhes: {'valor_total': 24567.45, 'assinatura_coerente': True, 'datas_coerentes': True, 'se
```
