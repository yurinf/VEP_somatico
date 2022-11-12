# VEP_somatico
Anotação de um VCF utilizando Ensembl VEP 105.0 

## ÍNDICE

---
# INTRODUÇÃO
O objetivo desse repositório é explorar, de forma prática, a anotação de um arquivo VCF utilizando o Ensembl VEP 105.0.

O Ensemble VEP é uma ferramenta que determina os efeitos de variantes em genes, transcritos e sequências proteicas. Confira mais no site do [Ensembl Variant Effect Predictor (VEP)](https://www.ensembl.org/info/docs/tools/vep/index.html).

Existem três interfaces de VEP ([Interface WEB](https://www.ensembl.org/info/docs/tools/vep/online/index.html), [linhas de comando](https://www.ensembl.org/info/docs/tools/vep/script/index.html) e [API](https://rest.ensembl.org/#VEP)). Aqui, será utilizado linhas de comando, e o ambiente [Google Colab](https://colab.research.google.com/) para executá-las em [Python](https://www.python.org/) no navegador.

# ACESSANDO O GOOGLE COLAB
Para utilizar o ambiente Google Colab, é necessário criar uma conta Google. Em seguida, acesse o [Google Colab](https://colab.research.google.com/) utilizando sua conta Google. Crie um novo notebook.

# PREPARANDO O AMBIENTE GOOGLE COLAB
Antes de inciar a anotação utilizando o Ensembl VEP, é necessário preparar o ambiente Google Colab montando o Google Drive (local na nuvem onde estarão os arquivos necessários para o ambiente funcionar corretamente), e instalar as ferramentas como Ensembl VEP e pandas.

## Montando o Google Drive
```python
from google.colab import drive
drive.mount('/content/drive')
```

## Instalando o Ensembl VEP 105.0
```python
%%bash
sudo apt install unzip curl git libmodule-build-perl libdbi-perl libdbd-mysql-perl build-essential zlib1g-dev
wget -c https://github.com/Ensembl/ensembl-vep/archive/refs/tags/105.0.tar.gz
tar -zxvf 105.0.tar.gz
cd ensembl-vep-105.0
./INSTALL.pl --NO_UPDATE 
```
🔴 **NOTA:** Estamos utilizando o Ensembl VEP **105.0**. Acesse o [GitHub do Ensemble](https://github.com/Ensembl/ensembl-vep/tags) para conferir as outra versões do Ensembl VEP.

Após a instalação do Ensembl VEP 105.0, é possível testar se a ferramenta foi instalada corretamente com o comando a seguir:
```python
%%bash
cd ensembl-vep-105.0
./vep
```

## Instalando o Pandas
```python
%%bash
pip install pandas
```

## Criando diretório no Google Drive para carregar o arquivo VCF
Criar uma pasta de nome **Dados** no Google Drive.
```python
mkdir /content/drive/MyDrive/Dados
```

## Carregando o arquivos para o Google Drive
Para essa prática utilizaremos os arquivos *WP312.filtered.vcf.gz* e *Homo_sapiens_assembly19.fasta* que se encontram neste repositório. 
1. Fazer o download dos arquivos *WP312.filtered.vcf.gz* e *Homo_sapiens_assembly19.fasta* na sua máquina local
2. Acessar o Google Drive com sua conta Google
3. Fazer upload dos arquivos *WP312.filtered.vcf.gz* e *Homo_sapiens_assembly19.fasta* na pasta **Dados** do seu Google Drive

# ANOTAÇÃO DO VCF COM ENSEMBL VEP 105.0
```python
%%bash
./ensembl-vep-105.0/vep  \
  --fork 3 \
	-i /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/105_GRCh37/WP312.filtered.vcf.gz \
	-o WP312.filtered.vcf.tsv \
  --dir_cache /content/drive/Shareddrives/T4-2022/ \
  --fasta /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/Homo_sapiens_assembly19.fasta \
  --cache --offline --assembly GRCh37 --refseq  \
	--pick --pick_allele --force_overwrite --tab --symbol --check_existing\
  --fields "Uploaded_variation,Location,Allele,Existing_variation,HGVSc,HGVSp,SYMBOL,Consequence,IND,ZYG,Amino_acids,CLIN_SIG,PolyPhen,SIFT,VARIANT_CLASS,FREQS" \
  --individual all
  ```
