# Hackathon IADT — Detecção Automática de Componentes e Relatório STRIDE

## 📌 Descrição
Este projeto implementa uma solução de **modelagem de ameaças automatizada** a partir de **diagramas de arquitetura de software em imagem**.  
A solução detecta automaticamente os **componentes do diagrama** (usuários, servidores, bancos de dados, APIs, etc.) e gera um **relatório de ameaças STRIDE** com vulnerabilidades e contramedidas.

O desenvolvimento foi feito como parte do **Hackathon IADT (FIAP)**, com foco em comprovar a viabilidade do uso de **Inteligência Artificial** para análise de arquiteturas de sistemas.

Aluno: Vinícius Oliveira Litran Andrade.
---

## 🎯 Objetivos
- Interpretar automaticamente diagramas de arquitetura de software.
- Detectar e classificar componentes de sistemas em imagens.
- Gerar um relatório de **modelagem de ameaças** baseado na metodologia **STRIDE**.
- Associar a cada componente detectado suas vulnerabilidades e contramedidas.

---

## 🧩 Arquitetura da Solução
1. **Dataset**  
   - Formato Pascal VOC (imagens `.png` + anotações `.xml`).  
   - Contém diagramas anotados com bounding boxes dos componentes.
   - Fonte: https://www.kaggle.com/datasets/carlosrian/software-architecture-dataset/data

2. **Treinamento**  
   - Modelo **Faster R-CNN (ResNet50-FPN)** pré-treinado no COCO.  
   - Fine-tuning com dataset anotado.  
   - Avaliação via **IoU médio** e **mAP simplificado**.

3. **Predição**  
   - Detecção de componentes em novas imagens.  
   - Geração de imagem anotada (`prediction.png`) e JSON estruturado (`prediction.json`).

4. **Relatório STRIDE**  
   - Cada componente detectado é associado às ameaças STRIDE correspondentes.  
   - O relatório final é salvo em `stride_report.json`.

---

## 📂 Estrutura de Pastas
├── dataset_augmented/ # Dataset (PNG + XML no formato Pascal VOC)

├── teste/ # Pasta com imagens de teste (.png)

├── output/ # Saídas (modelos, predições e relatórios)

│ ├── epochs/ # Checkpoints por época

│ ├── prediction.png # Imagem anotada

│ ├── prediction.json # Predições em JSON

│ └── stride_report.json # Relatório STRIDE final

└── notebook.ipynb # Notebook principal do projeto

---

## ⚙️ Como Executar

### 1. Pré-requisitos
- Python 3.9+
- PyTorch + Torchvision
- OpenCV, NumPy, tqdm, Pillow

Instale dependências:

pip install torch torchvision opencv-python tqdm pillow

### 2. Preparar Dataset

Coloque seu dataset anotado em dataset_augmented/ (arquivos .png + .xml).

### 3. Rodar Notebook

Abra e execute o notebook.ipynb no Jupyter, ou rode como script:
python notebook.py

### 4. Predição de Teste

Coloque ao menos uma imagem .png na pasta teste/.
A saída incluirá:

prediction.png (imagem com bounding boxes)

prediction.json (resultados de detecção)

stride_report.json (ameaças STRIDE associadas)

📊 Exemplo de Saída
Imagem anotada

JSON de Predição:
{
  "predictions": [
    {
      "confidence": 0.95,
      "displayName": "aws_amazon_ec2",
      "boundingBox": { "xMin": 0.12, "yMin": 0.20, "xMax": 0.45, "yMax": 0.60 }
    }
  ],
  "modelInfo": {
    "type": "local_pytorch",
    "model_path": "output/soft_arch_final_xxxxx.pth"
  }
}

Relatório STRIDE:
{
  "components": [
    {
      "component": "aws_amazon_ec2",
      "confidence": 0.95,
      "threats": [
        {
          "category": "S",
          "name": "Spoofing (Falsificação de Identidade)",
          "vulnerabilities": ["Credenciais fracas", "Falta de autenticação multifator"],
          "countermeasures": ["Implementar MFA", "Política de senhas fortes", "Certificados digitais"]
        }
      ]
    }
  ]
}


---

🔐 Metodologia STRIDE

S: Spoofing (Falsificação de Identidade)

T: Tampering (Violação de Integridade)

R: Repudiation (Repúdio)

I: Information Disclosure (Divulgação de Informação)

D: Denial of Service (Negação de Serviço)

E: Elevation of Privilege (Elevação de Privilégios)

Cada componente identificado é mapeado para uma ou mais categorias e recebe vulnerabilidades e contramedidas específicas.

---

📽️ Entregáveis

Notebook completo (.ipynb) com o pipeline. (Litran_Hackathon.ipynb)

README.md (este documento).

Vídeo (até 15 min) demonstrando a solução (execução e explicação).
