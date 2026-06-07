<p align="center">
  <img src="https://img.shields.io/badge/CLUSTER-HEALTH:_GREEN-3fb950?style=for-the-badge&logo=elastic&logoColor=white&labelColor=0d1117">
  <img src="https://img.shields.io/badge/NODES-1-8b949e?style=for-the-badge&labelColor=0d1117">
  <img src="https://img.shields.io/badge/INDICES-3-8b949e?style=for-the-badge&labelColor=0d1117">
</p>

```http
GET /engineers,contributions,certs/_search
```

```json
{
  "query": {
    "bool": {
      "must": [
        { "term": { "github.keyword": "vigneshio" }}
      ],
      "should": [
        { "match": { "skills": "elasticsearch lucene java search" }},
        { "match": { "certs": "elastic google cloud oracle aws" }},
        { "match": { "bio": "open source contributor squareshift" }}
      ],
      "minimum_should_match": 1
    }
  },
  "aggs": {
    "certifications": { "terms": { "field": "certs.keyword", "size": 10 }},
    "tech_stack": { "terms": { "field": "skills.keyword", "size": 15 }},
    "cloud_platforms": { "terms": { "field": "cloud.keyword", "size": 5 }}
  },
  "highlight": {
    "fields": {
      "bio": { "fragment_size": 150 },
      "role": {}
    }
  },
  "sort": [{ "_score": "desc" }],
  "size": 1
}
```

---

### Response

```json
{
  "took": 27,
  "timed_out": false,
  "_shards": {
    "total": 3,
    "successful": 3,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 1,
      "relation": "eq"
    },
    "max_score": 99.99,
    "hits": [
      {
        "_index": "engineers",
        "_id": "vignesh-a",
        "_score": 99.99,
        "_source": {
          "name": "Vignesh A",
          "handle": "vigneshio",
          "pronouns": "He/Him",
          "role": "Engineer @ SquareShift",
          "location": {
            "city": "Greater Chennai Area",
            "country": "IN"
          },
          "education": "Kalasalingam University",
          "connections": "500+",
          "status": "open_to_contributions",
          "warrior_award": {
            "issuer": "SquareShift",
            "reason": "Did not quit when deadlines got tight",
            "year": 2025
          },
          "bio": "Building search and data infrastructure. Spent more nights than I care to admit debugging production clusters at 2 AM. Got hooked on Elasticsearch after my first on-call rotation and kept going until I had four Elastic certs. Currently contributing to the same open source projects I used to read documentation for."
        },
        "highlight": {
          "role": [
            "Engineer at <em>SquareShift</em>"
          ],
          "bio": [
            "Building <em>search</em> and <em>data</em> infrastructure",
            "debugging <em>production clusters</em>",
            "four <em>Elastic</em> certs",
            "contributing to <em>open source</em>"
          ]
        }
      }
    ]
  }
}
```

---

### Aggregations

```json
{
  "aggregations": {
    "certifications": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        { "key": "Elastic Certified Engineer", "doc_count": 1 },
        { "key": "Elastic Certified Observability Engineer", "doc_count": 1 },
        { "key": "Elastic Certified SIEM Analyst", "doc_count": 1 },
        { "key": "Elastic GenAI Associate", "doc_count": 1 },
        { "key": "Google Professional Cloud Architect", "doc_count": 1 },
        { "key": "Google Professional ML Engineer", "doc_count": 1 },
        { "key": "Google Professional Data Engineer", "doc_count": 1 },
        { "key": "Oracle AI Vector Search Professional", "doc_count": 1 },
        { "key": "AWS Certified Cloud Practitioner", "doc_count": 1 }
      ]
    },
    "tech_stack": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        { "key": "elasticsearch", "doc_count": 1 },
        { "key": "java", "doc_count": 1 },
        { "key": "python", "doc_count": 1 },
        { "key": "sql", "doc_count": 1 },
        { "key": "apache-kafka", "doc_count": 1 },
        { "key": "apache-spark", "doc_count": 1 },
        { "key": "apache-airflow", "doc_count": 1 },
        { "key": "kubernetes", "doc_count": 1 },
        { "key": "docker", "doc_count": 1 },
        { "key": "prometheus", "doc_count": 1 },
        { "key": "grafana", "doc_count": 1 },
        { "key": "linux", "doc_count": 1 }
      ]
    },
    "cloud_platforms": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        { "key": "aws", "doc_count": 1 },
        { "key": "gcp", "doc_count": 1 }
      ]
    }
  }
}
```

---

### Mappings

```json
PUT /_index_template/engineer_vignesh
{
  "index_patterns": ["engineers"],
  "template": {
    "settings": {
      "number_of_shards": 1,
      "number_of_replicas": 0
    },
    "mappings": {
      "dynamic": "strict",
      "properties": {
        "name": { "type": "keyword" },
        "handle": { "type": "keyword" },
        "role": { "type": "text", "analyzer": "standard" },
        "location": {
          "properties": {
            "city": { "type": "keyword" },
            "country": { "type": "keyword" }
          }
        },
        "skills": { "type": "keyword" },
        "certs": { "type": "keyword" },
        "cloud": { "type": "keyword" },
        "bio": { "type": "text", "analyzer": "standard" },
        "status": { "type": "keyword" },
        "warrior_award": {
          "properties": {
            "issuer": { "type": "keyword" },
            "reason": { "type": "text" },
            "year": { "type": "integer" }
          }
        }
      }
    }
  }
}
```

---

### Related Documents (Open Source)

```json
{
  "took": 5,
  "timed_out": false,
  "hits": {
    "total": { "value": 3, "relation": "eq" },
    "hits": [
      {
        "_index": "contributions",
        "_id": "apache-lucene",
        "_source": {
          "repo": "apache/lucene",
          "type": "contributor",
          "status": "active",
          "focus": "search-engineering"
        }
      },
      {
        "_index": "contributions",
        "_id": "elastic-elasticsearch",
        "_source": {
          "repo": "elastic/elasticsearch",
          "type": "contributor",
          "status": "active",
          "focus": "distributed-search"
        }
      },
      {
        "_index": "contributions",
        "_id": "apache-polaris",
        "_source": {
          "repo": "apache/polaris",
          "type": "contributor",
          "status": "pending_first_merge",
          "focus": "catalog-federation"
        }
      }
    ]
  }
}
```

---

### Routing & Links

```json
{
  "_links": {
    "self": "https://github.com/vigneshio",
    "linkedin": {
      "href": "https://www.linkedin.com/in/avignesh27/",
      "title": "LinkedIn"
    },
    "x": {
      "href": "https://x.com/im_viGnesh27",
      "title": "X / Twitter"
    },
    "email": {
      "href": "mailto:imavignesh27@gmail.com",
      "title": "imavignesh27@gmail.com"
    }
  }
}
```

---

<p align="center">
  <img src="https://komarev.com/ghpvc/?username=vigneshio&color=blueviolet&style=flat-square&label=INDEX+REQUESTS">
</p>
