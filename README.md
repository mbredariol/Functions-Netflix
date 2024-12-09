# Functions-Netflix
Criando um Gerenciador de Catálogos da Netflix com Azure Functions e Banco de Dados

Criar um Gerenciador de Catálogos da Netflix usando Azure Functions e um banco de dados envolve o desenvolvimento de um backend serverless para gerenciar informações sobre filmes, séries, gêneros, e mais, integrado com um banco de dados escalável e gerenciado.
________________________________________
Arquitetura
1.	Azure Functions: Fornece APIs serverless para criar, atualizar, deletar e buscar informações do catálogo.
2.	Banco de Dados: Use Azure Cosmos DB (NoSQL) ou Azure SQL Database (relacional) para armazenar dados do catálogo.
3.	Storage para Imagens: Utilize Azure Blob Storage para armazenar pôsteres ou miniaturas de filmes/séries.
4.	Gateway de API: Opcionalmente, utilize o Azure API Management para proteger e gerenciar as APIs.
5.	Aplicativo Frontend: (opcional) Um app web ou móvel pode consumir as APIs criadas.
________________________________________
Passo a Passo
1. Criação do Banco de Dados
•	Azure Cosmos DB (NoSQL): 
o	Modelo de documento (JSON).
o	Estrutura sugerida: 
o	{
o	  "id": "unique-id",
o	  "title": "Movie Title",
o	  "description": "Short Description",
o	  "genres": ["Action", "Drama"],
o	  "releaseYear": 2023,
o	  "posterUrl": "https://yourblob.blob.core.windows.net/posters/movie-id.jpg"
o	}
•	Azure SQL Database: 
o	Tabelas sugeridas: 
	Movies: Armazena informações dos filmes/séries.
	Genres: Lista os gêneros disponíveis.
	Relacionamento MovieGenres para filmes com múltiplos gêneros.
________________________________________
2. Configuração das Azure Functions
•	Criação da Function App:
o	Linguagens suportadas: C#, Python, JavaScript, ou outra de sua escolha.
o	Triggers: 
	HTTP Trigger: APIs RESTful.
	Timer Trigger: Tarefas agendadas (ex.: limpeza de catálogos antigos).
•	Endpoints RESTful:
Verbo HTTP	Endpoint	Função
GET	/movies	Retorna todos os filmes.
GET	/movies/{id}	Retorna detalhes de um filme.
POST	/movies	Adiciona um novo filme ao catálogo.
PUT	/movies/{id}	Atualiza informações de um filme.
DELETE	/movies/{id}	Remove um filme do catálogo.
•	Exemplo de implementação:
o	Em Python, usando Azure Functions com o Cosmos DB: 
o	import azure.functions as func
o	import json
o	from azure.cosmos import CosmosClient
o	
o	client = CosmosClient("https://<your-account>.documents.azure.com", "<your-key>")
o	database = client.get_database_client("CatalogDB")
o	container = database.get_container_client("Movies")
o	
o	def main(req: func.HttpRequest) -> func.HttpResponse:
o	    if req.method == "GET":
o	        movies = list(container.read_all_items())
o	        return func.HttpResponse(json.dumps(movies), status_code=200, mimetype="application/json")
o	    elif req.method == "POST":
o	        new_movie = req.get_json()
o	        container.create_item(new_movie)
o	        return func.HttpResponse("Movie added!", status_code=201)
________________________________________
3. Armazenamento de Imagens
•	Configure um Azure Blob Storage: 
o	Crie um container para armazenar imagens dos filmes.
o	Gera URLs de acesso público ou assinado para os pôsteres.
•	Utilize SDKs do Azure para upload de imagens.
________________________________________
4. Segurança e Monitoramento
•	Segurança: 
o	Configure autenticação nas APIs usando Azure AD ou JWT.
o	Use Azure API Management para rate limiting e proteção contra abusos.
•	Monitoramento: 
o	Configure logs e métricas com Azure Application Insights.
________________________________________
5. Escalabilidade
•	As Azure Functions escalam automaticamente com base no tráfego.
•	O Cosmos DB ou SQL Database suporta escalabilidade para demandas maiores.
________________________________________
6. Testes e Deploy
•	Localmente, teste as Azure Functions usando o Azure Functions Core Tools.
•	Faça o deploy com o comando: 
•	func azure functionapp publish <your-function-app-name>
________________________________________


