- Azure AI Vision - Análise de Imagem

Este projeto criará um recurso autônomo de Visão Computacional. Pode-se usar os serviços de Visão Computacional do Azure AI em um recurso multisserviço dos Serviços de IA do Azure, diretamente ou em um projeto do Azure AI Foundry. Será utilizado um aplicativo cliente que usará o Azure AI Vision SDK para analisar imagens.

Será criado:

1. Recurso do Azure AI Vision
2. Aplicativo de análise de imagens com o Azure AI Vision SDK

- Como usar: Passo a passo

1. Acesse o Portal do Azure Estúdio
   [https://azure.microsoft.com]

2. Provisionando recurso no Azure AI Vision
   Selecione "+Criar Recurso"
   Na barra de pesquisa, busque por "Computer Vision"e crie um recurso
   Preencha: Assinatura, Grupo de Recursos, Região, Nome e Nível de preço
   Clique em "Criar e revisar" e após verificação, em "Criar"
   Após a criação, acesse o recurso e na aba lateral "Gerenciamento de Recursos" e copie  

3. Desenvolvendo um aplicativo de análise de imagens com o Azure AI Vision SDK
   Crie um novo Cloud Shell no portal do Azure, selecionando o ambiente do PowerShell sem armazenamento na sua assinatura. 
   Na barra de ferramentas do Cloud Shell, em configurações, selecione "ir para a versão clássica
   Clone o repositório do GitHub que contém os arquivos de código
   "rm -r mslearn-ai-vision -f
    git clone https://github.com/MicrosoftLearning/mslearn-ai-vision"
   Visualize a pasta com os arquivos clonados
   "cd mslearn-ai-vision/Labfiles/analyze-images/python/image-analysis
    ls -a -l"
   Instale o pacote Azure AI Vision SDK e outros pacotes necessários
   "python -m venv labenv
    ./labenv/bin/Activate.ps1
    pip install -r requirements.txt azure-ai-vision-imageanalysis==1.0.0"
    Edite o arquivo de configuração
    "Code .env"
    O arquivo é aberto em um editor de código
    Copie o endpoint e a chave nos lugares solicitados
    Use CTRL+S para salvar e CTRL+Q para sair
   
4. Sugerindo legenda através de código
   Na linha de comando do Cloud Shell abra o arquivo de código do cliente
   "code image-analysis.py"
   Encontre o comentário Import namespaces e adicione o seguinte código para importar os namespaces que você precisará para usar o Azure AI Vision SDK:
   "from azure.ai.vision.imageanalysis import ImageAnalysisClient
    from azure.ai.vision.imageanalysis.models import VisualFeatures
    from azure.core.credentials import AzureKeyCredential"
    Localize o comentário " Authenticate Azure AI Vision client" e adicione o seguinte código para criar e autenticar um objeto de cliente do Azure AI Vision:
    "cv_client = ImageAnalysisClient(
          endpoint=ai_endpoint,
          credential=AzureKeyCredential(ai_key))"
    Encontre o comentário Analisar imagem e adicione o seguinte código:
    "with open(image_file, "rb") as f:
          image_data = f.read()
     print(f'\nAnalyzing {image_file}\n')

     result = cv_client.analyze(
          image_data=image_data,
          visual_features=[
              VisualFeatures.CAPTION,
              VisualFeatures.DENSE_CAPTIONS,
              VisualFeatures.TAGS,
              VisualFeatures.OBJECTS,
              VisualFeatures.PEOPLE],
    )
    Encontre o comentário Obter legendas de imagens , adicione o seguinte código para exibir legendas de imagens e legendas densas:
    "if result.caption is not None:
          print("\nCaption:")
          print(" Caption: '{}' (confidence: {:.2f}%)".format(result.caption.text, result.caption.confidence * 100))
    
     if result.dense_captions is not None:
          print("\nDense Captions:")
          for caption in result.dense_captions.list:
              print(" Caption: '{}' (confidence: {:.2f}%)".format(caption.text, caption.confidence * 100))
     Salve e teste com o comando no Cloud Shell: python image-analysis.py images/street.jpg

5. Adicionando código para gerar tags sugeridas
   No editor de código, na função AnalyzeImage , encontre o comentário Obter tags de imagem e adicione o seguinte código:
   "if result.tags is not None:
         print("\nTags:")
         for tag in result.tags.list:
             print(" Tag: '{}' (confidence: {:.2f}%)".format(tag.name, tag.confidence * 100))  
   Salve as alterações e execute o programa com o argumento images/street.jpgClique em "Criar" e escolha "arquivo local"
  
6. Adicionando código para detectar e localizar objetos
   No editor de código, na função AnalyzeImage, adicione o seguinte código para listar os objetos detectados na imagem e chame a função fornecida para anotar uma imagem com os objetos detectados:
   "if result.objects is not None:
         print("\nObjects in image:")
         for detected_object in result.objects.list:
             # Print object tag and confidence
             print(" {} (confidence: {:.2f}%)".format(detected_object.tags[0].name, detected_object.tags[0].confidence * 100))
         # Annotate objects in the image
         show_objects(image_file, result.objects.list)"
   Salve e execute o programa com o argumento images/street.jpg
   Após a execução, é gerado um arquivo chamado objects.jpg
   Use o comando "download objects.jpg"no Shell, para baixar o arquivo
   Execute o comando para todo o arquivo
    
7. Adicione código para detectar e localizar pessoas
    No editor de código, na função AnalyzeImage, adicione o seguinte código para listar as pessoas detectadas com um nível de 20 % de segurança ou mais:
    "if result.people is not None:
          print("\nPeople in image:")

          for detected_person in result.people.list:
              if detected_person.confidence > 0.2:
                  # Print location and confidence of each person detected
                  print(" {} (confidence: {:.2f}%)".format(detected_person.bounding_box,     detected_person.confidence * 100))
          # Annotate people in the image
          show_people(image_file, result.people.list)"
    Salve e execute o programa com o argumento images/street.jpg
    Use o comando "download people.jpg"no Shell, para baixar o arquivo
    Execute o comando para todo o arquivo       
 
- Tecnologias Usadas

 1. Microsoft Azure - (https://azure.microsoft.com/)
 2. Azure AI Vision - (https://azure.microsoft.com/)
 3. ARM Templates - (https://learn.microsoft.com/azure/azure-resource-manager/templates/overview)
 4. Markdown para documentação

- Autor
  Dannuza Martins Cardoso Silva
 
 


