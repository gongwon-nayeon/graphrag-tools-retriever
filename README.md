# graphrag-tools-retriever

Neo4j GraphRAG 기반 뉴스 기사 검색 시스템

네이버 뉴스 기사를 수집하여 Neo4j 그래프 데이터베이스에 저장하고, Vector, VectorCypher, Text2Cypher 검색을 통합한 ToolsRetriever 기반 GraphRAG 시스템을 구축합니다.

## 주요 기능

1. **데이터 수집 (DataScrapping.ipynb)**
   - 네이버 뉴스 카테고리별 기사 크롤링 (정치, 경제, 사회, 생활/문화, IT/과학, 세계)
   - Selenium 기반 동적 웹 페이지 스크래핑
   - Excel 형식으로 기사 정보 저장

2. **그래프 DB 구축 (GraphBuilder.ipynb)**
   - 수집된 기사 데이터를 Neo4j에 적재
   - 기사 내용을 청크 단위로 분할하여 Content 노드 생성
   - Article, Content, Media, Category 노드 및 관계 생성
   - OpenAI Embeddings를 활용한 벡터 인덱스 구축

3. **GraphRAG 검색 (ToolsRetriever.ipynb)**
   - **Vector Retriever**: Content 노드 기반 의미론적 유사도 검색
   - **VectorCypher Retriever**: 벡터 검색 + 카테고리 기반 복합 검색
   - **Text2Cypher Retriever**: 자연어 질의를 Cypher 쿼리로 변환한 구조적 검색
   - **ToolsRetriever**: 3가지 검색 도구를 통합하여 최적의 검색 방식 자동 선택


```
graphrag-tools-retriever/

├── DataScrapping.ipynb      # 데이터 수집
├── GraphBuilder.ipynb       # 그래프 구축
├── ToolsRetriever.ipynb     # GraphRAG
├── .env.example
└── requirements.txt
```

## 그래프 DB 스키마

<img width="1996" height="675" alt="Knowledge Graph" src="https://github.com/user-attachments/assets/652e97c2-9de7-493a-b609-eb9b4af387f8" />


- **Article** 노드: 기사 정보 (article_id, title, url, published_date)
- **Content** 노드: 기사 내용 청크 (chunk, article_id, title, url, published_date, chunk_index)
- **Media** 노드: 언론사 정보 (name)
- **Category** 노드: 카테고리 정보 (name)

### 관계

- Article -[HAS_CHUNK]-> Content
- Media -[PUBLISHED]-> Article
- Article -[BELONGS_TO]-> Category

## 설치 방법


```bash
uv venv
uv pip install -r requirements.txt
 .venv\Scripts\activate
```

## 참조 자료

[Neo4j GraphRAG 파이썬 패키지 위키독스 ToolsRetriever](https://wikidocs.net/307512)
