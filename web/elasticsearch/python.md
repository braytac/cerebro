https://elasticsearch-dsl.readthedocs.io/en/latest/index.html
Por debajo hace uso de https://github.com/elastic/elasticsearch-py

pip install elasticsearch-dsl

# Query
from elasticsearch_dsl import Search
from elasticsearch_dsl.connections import connections
hosts = ["http://localhost:9200"]
connections.create_connection(hosts=hosts, timeout=self.args.elastic_timeout)

s = Search(index="metricbeat-*").filter("term", metricset__name="memory") \
                                .filter("term", metricset__module="system") \
                                .query('range', ** {'@timestamp': {'gt': f"now-1h"}})


Search(index="metricbeat-*").filter("term", metricset__name="core").filter("term", metricset__module="system")
Si tenemos que poner un "." usaremos "__"

Fecha con @timestamp:
Search(index="metricbeat-*").query('range', ** {'@timestamp': {'gt': 'now-10m'}})

# Agregaciones
from elasticsearch_dsl.aggs import A
s = Search(index="metricbeat-*")
s.aggs.bucket("cpu_per_host", "terms", field="beat.hostname").metric("user", "avg", field="system.core.user.pct").metric("sys", "avg", field="system.core.system.pct").metric("iowait", "avg", field="system.core.iowait.pct")

s.to_dict()
Para ver la query que vamos a lanzar


# Ejecutar
response = s.execute()
response.success()
  chequea que el número de shards totales sea igual al de successful (puede diferir si tenemos problemas en el cluster) y no haya timeout




# Respuestas
response = s.execute()

## Hits
print('Total %d hits found.' % response.hits.total)
for h in response:
    print(h.title, h.body)


## Agregaciones
for tag in response.aggregations.per_tag.buckets:
    print(tag.key, tag.max_lines.value)




# Mappings
Crear mappings: http://elasticsearch-dsl.readthedocs.io/en/latest/persistence.html

Crear a mano (habremos definido la conex antes):
from elasticsearch_dsl import Index
i = Index("adrires2")
i.put_mapping(doc_type="doc", body={"properties": {"@timestamp": {"type": "date"}}})


# Index
Crear index: http://elasticsearch-dsl.readthedocs.io/en/latest/persistence.html#index

Almacenar algo en el index usando la lib elasticsearch de por debajo.
host = "http://elastic:elastic@127.0.0.1"
connections.create_connection(hosts=host)
es = connections.get_connection()
es.index(index="adrires",doc_type='doc',body={"una":1})



# Date
Usar el formato de fecha de python
"@timestamp": date


# Bulk
https://elasticsearch-py.readthedocs.io/en/master/helpers.html#bulk-helpers
def gendata():
    mywords = ['foo', 'bar', 'baz']
    for word in mywords:
        yield {
            "_index": "mywords",
            "_type": "document",
            "doc": {"word": word},
        }

bulk(es, gendata())
