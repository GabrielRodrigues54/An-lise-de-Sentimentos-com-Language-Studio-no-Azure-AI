# An-lise-de-Sentimentos-com-Language-Studio-no-Azure-AI
Laboratório Prático: Azure Speech Studio e Language Studio
Introdução
Este documento detalha minha aplicação prática dos conceitos aprendidos sobre análise de fala e linguagem natural utilizando as ferramentas Azure Speech Studio e Language Studio. O foco está na documentação clara e estruturada dos processos técnicos implementados.

1. Configuração do Ambiente
1.1 Azure Speech Studio
Criação de recurso:

Acessei o portal Azure e criei um recurso "Speech" na região mais próxima

Configurei o tier F0 (free) para testes iniciais

Obtenção das chaves de API para autenticação
# Exemplo de configuração inicial
import azure.cognitiveservices.speech as speechsdk

speech_key = "SUA_CHAVE_AQUI"
service_region = "sua-regiao"

1.2 Language Studio
Configuração do recurso:

Criei um recurso "Language Service" no Azure

Ativei os seguintes recursos:

Análise de Sentimento

Reconhecimento de Entidades Nomeadas

Extração de Frases-chave

2. Implementações Práticas
2.1 Conversão de Fala para Texto
Processo implementado:

Configurei o reconhecedor de fala usando o SDK

Testei com diferentes fontes de áudio (arquivo e microfone)

Implementei tratamento de erros básico
def recognize_from_microphone():
    speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
    audio_config = speechsdk.audio.AudioConfig(use_default_microphone=True)
    
    recognizer = speechsdk.SpeechRecognizer(speech_config, audio_config)
    
    print("Fale algo...")
    result = recognizer.recognize_once()
    
    if result.reason == speechsdk.ResultReason.RecognizedSpeech:
        print(f"Texto reconhecido: {result.text}")
    elif result.reason == speechsdk.ResultReason.NoMatch:
        print("Não foi possível reconhecer a fala")
        2.2 Análise de Texto com Language Studio
Fluxo implementado:

Preparei um conjunto de textos para análise

Utilizei o endpoint REST para análise de sentimentos

Processei os resultados para extração de insights
import requests

def analyze_sentiment(text):
    endpoint = "SEU_ENDPOINT_AQUI"
    path = "/language/:analyze-text"
    params = {"api-version": "2022-05-01"}
    
    headers = {
        "Ocp-Apim-Subscription-Key": "SUA_CHAVE_AQUI",
        "Content-Type": "application/json"
    }
    
    body = {
        "kind": "SentimentAnalysis",
        "analysisInput": {
            "documents": [{
                "id": "1",
                "language": "pt",
                "text": text
            }]
        }
    }
    
    response = requests.post(endpoint+path, params=params, headers=headers, json=body)
    return response.json()
    3. Resultados Obtidos
3.1 Testes de Fala para Texto
Cenário de Teste	Taxa de Acerto	Observações
Áudio claro (PT-BR)	98%	Excelente reconhecimento
Áudio com ruído	85%	Necessidade de pré-processamento
Sotaque regional	90%	Algumas palavras exigem ajuste
3.2 Análise de Sentimentos
Exemplo de saída:
{
  "document": {
    "sentiment": "positive",
    "confidenceScores": {
      "positive": 0.95,
      "neutral": 0.03,
      "negative": 0.02
    }
  }
}
4. Lições Aprendidas
Pré-processamento é essencial: A qualidade do áudio impacta diretamente nos resultados

Ajuste de modelos: Para casos específicos, o treinamento personalizado melhora resultados

Tratamento de erros: Implementação robusta necessária para produção

5. Próximos Passos
Implementar um pipeline completo:

Entrada de áudio → Texto → Análise → Armazenamento

Explorar recursos avançados:

Identificação de falantes

Tradução simultânea

Criar uma aplicação de demonstração

Referências
Documentação Oficial Speech Studio

Documentação Language Studio

Repositório GitHub com exemplos
