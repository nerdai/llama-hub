# Patronus AI FinanceBench Dataset

[![patronus-ai-logo (200 x 40 px)](https://github.com/nerdai/llama-hub/assets/92402603/62a6df3f-57a3-4d68-917b-b0947392efcd)](https://www.patronus.ai/)

This dataset is a subset of the original FinanceBench dataset. In particular, to
make this benchmark more computationally efficient, we only keep the documents for
which there are 2 or more questions. Such filtering, reduced the total unique pdf
documents from 98 to 32.

## CLI Usage

You can download `llamadatasets` directly using `llamaindex-cli`, which comes installed with the `llama-index` python package:

```bash
llamaindex-cli download-llamadataset PatronusAIFinanceBenchDataset --download-dir ./data
```

You can then inspect the files at `./data`. When you're ready to load the data into
python, you can use the below snippet of code:

```python
from llama_index import SimpleDirectoryReader
from llama_index.llama_dataset import LabelledRagDataset

rag_dataset = LabelledRagDataset.from_json("./data/rag_dataset.json")
documents = SimpleDirectoryReader(
    input_dir="./data/source_files"
).load_data()
```

## Code Usage

You can download the dataset to a directory, say `./data` directly in Python
as well. From there, you can use the convenient `RagEvaluatorPack` llamapack to
run your own LlamaIndex RAG pipeline with the `llamadataset`.

```python
from llama_index.llama_dataset import download_llama_dataset
from llama_index.llama_pack import download_llama_pack
from llama_index import VectorStoreIndex

# download and install dependencies for benchmark dataset
rag_dataset, documents = download_llama_dataset(
  "PatronusAIFinanceBenchDataset", "./data"
)

# build basic RAG system
index = VectorStoreIndex.from_documents(documents=documents)
query_engine = index.as_query_engine()

# evaluate using the RagEvaluatorPack
RagEvaluatorPack = download_llama_pack(
  "RagEvaluatorPack", "./rag_evaluator_pack"
)
rag_evaluator_pack = RagEvaluatorPack(
    rag_dataset=rag_dataset,
    query_engine=query_engine
)
benchmark_df = rag_evaluator_pack.run()  # async arun() supported as well
```
