# Labeled Images Dataset for Computer Vision

## Overview
Conjunto de imagens rotuladas destinado ao treino e avaliação de modelos de computer vision. Contém imagens organizadas por classe e anotações em formatos comuns (COCO JSON e/ou Pascal VOC XML e/ou YOLO TXT). Inclui exemplos de carregamento, recomendações de uso e metadados essenciais.

## Estrutura sugerida
- data/
  - images/                ; ficheiros de imagem (.jpg, .png)
  - annotations/
    - coco_annotations.json
    - voc_annotations/     ; ficheiros .xml por imagem (opcional)
    - yolo_labels/         ; ficheiros .txt por imagem (opcional)
- docs/
  - LICENSE
  - README.md
  - CITATION.cff
  - datapackage.json
  - metadata.yaml
- examples/
  - load_dataset.py
- .gitattributes
- .gitignore

## Anotação e formatos suportados
- COCO JSON: deteção, segmentação, categorias e bounding boxes. Recomenda-se como formato primário para datasets complexos.
- Pascal VOC XML: ficheiro .xml por imagem com bounding boxes e classes.
- YOLO TXT: ficheiro .txt por imagem com linhas no formato: <class_id> <x_center> <y_center> <width> <height> (valores normalizados).

## Convenções de ficheiros
- Nomes de imagens sem espaços, usar snake_case ou kebab-case.
- IDs únicos por imagem; mantenha correspondência exata entre images/ e annotations/.
- Para legendas por classe em subpastas, usar:
  - data/images/class_name/image001.jpg (opcional, facilita inspeção)

## Exemplo mínimo COCO (esquemático)
- images: lista de objetos com id, file_name, width, height
- annotations: id, image_id, category_id, bbox, area, iscrowd
- categories: id, name, supercategory

## Carregar dataset em Python (exemplo COCO)
```python
from pycocotools.coco import COCO
from PIL import Image
import os

coco = COCO("data/annotations/coco_annotations.json")
cat_ids = coco.getCatIds()
img_ids = coco.getImgIds(catIds=cat_ids)
img_info = coco.loadImgs(img_ids[0])[0]
img_path = os.path.join("data/images", img_info["file_name"])
img = Image.open(img_path)
ann_ids = coco.getAnnIds(imgIds=img_info["id"])
anns = coco.loadAnns(ann_ids)
print(img_info, anns)
