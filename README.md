# README: Algoritmo de Roteamento Semântico com Base em Similaridade de Vetores

## Visão Geral

Este projeto contém a implementação de um "Roteador Semântico", um algoritmo desenhado para rotear solicitações para o serviço ou endpoint mais apropriado, com base na similaridade semântica da consulta de entrada utilizando embeddings de vetores.

### Descrição do Algoritmo

O algoritmo segue os seguintes passos principais:

1. **Gerar Embedding da Consulta**: A consulta é convertida em um embedding (representação vetorial) usando um modelo de embeddings.
2. **Buscar Similaridades**: O embedding da consulta é comparado com um conjunto de embeddings pré-existentes no banco de dados para encontrar as correspondências mais próximas.
3. **Agrupar por Rota**: As correspondências mais próximas são agrupadas pelas rotas ou serviços a que pertencem.
4. **Filtrar Pontuações**: O algoritmo filtra as rotas que não atendem a um determinado limite, com base na soma de suas pontuações de similaridade.
5. **Selecionar a Melhor Rota**: Por fim, a rota com a maior pontuação de similaridade é escolhida para lidar com a consulta.

## Estrutura do Projeto

O projeto está dividido em diferentes módulos, que são detalhados a seguir:

### Módulos Nativos ou Instalados

- `uuid`: Utilizado para gerar identificadores únicos universais.
- `typing`: Utilizado para fornecer dicas de tipo e literais.
- `sqlmodel`: Utilizado para manipulação e consulta de dados em um banco de dados SQL.

### Módulos do Usuário

- **database.py**: Contém a configuração e o motor do banco de dados (usando SQLAlchemy).
- **models.py**: Define os modelos de dados usados no SQLModel.
- **utils.py**: Contém funções utilitárias, incluindo a função de embedding.

### Exemplo de Implementação

A seguir, apresentamos um exemplo simples de como implementar e executar o algoritmo de roteamento semântico:

```python
import uuid
from typing import Literal
from sqlmodel import Session, select
from database import pg_engine # SQLAlchemy engine
from models import Embedding, Collection # SQLModel models
from utils import embed # Embedding function

def _set_collection_id(session: Session, layer: Literal['entry_layer', 'customer_layer']) -> uuid.UUID:
    collections = session.exec(
        select(Collection)
    ).all()
    
    collection_ids = {collection.name: collection.uuid for collection in collections}

    if layer == 'entry_layer':
        return collection_ids['routes']
    elif layer == 'customer_layer':
        return collection_ids['customer_routes']

query = "create a new customer in ge3"
layer = "entry_layer"
threshold = 0.5

vector = embed(query)

# Aqui você adicionaria o código para buscar similaridades, agrupar por rota,
# filtrar pontuações e selecionar a melhor rota.
```

### Dependências

Certifique-se de instalar todas as dependências necessárias utilizando o seguinte comando:

```bash
pip install -r requirements.txt
```

### Executando o Projeto

Para rodar o projeto, execute o notebook `semantic_router.ipynb` em um ambiente Jupyter, ou integre o código em seu serviço conforme necessário.

### Considerações Finais

O objetivo deste projeto é oferecer uma forma eficiente de rotear solicitações para serviços ou endpoints específicos, com base na similaridade semântica, melhorando a precisão e eficiência da resposta. 


---

**Nota:** Este documento foi gerado para fornecer uma compreensão clara e concisa do funcionamento e implementação do algoritmo de roteamento semântico. Certifique-se de revisar e adaptar o código conforme suas necessidades específicas.

