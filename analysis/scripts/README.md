# filter_bottlerocket_amis.sh

Script para buscar AMIs do Bottlerocket OS na AWS via CLI, filtrando por versão do Bottlerocket e versão do Kubernetes.

## Pré-requisitos

- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) configurado com credenciais válidas
- [jq](https://stedolan.github.io/jq/download/) instalado
- Permissão IAM: `ec2:DescribeImages` e `ec2:DescribeRegions`

## Padrão de filtro

O script busca AMIs com o seguinte padrão de nome:

```
bottlerocket-aws-k8s-<K8S_VERSION>-*-v<BOTTLEROCKET_VERSION>-*
```

**Exemplo de AMI encontrada:**
```
bottlerocket-aws-k8s-1.35-nvidia-fips-x86_64-v1.58.0-c5c37032
```

## Uso

```bash
./filter_bottlerocket_amis.sh [BOTTLEROCKET_VERSION] [K8S_VERSION] [REGIOES]
```

### Argumentos

| Argumento             | Descrição                                      | Padrão  |
|-----------------------|------------------------------------------------|---------|
| `BOTTLEROCKET_VERSION` | Versão do Bottlerocket OS                     | `1.58.0` |
| `K8S_VERSION`          | Versão do Kubernetes                          | `1.35`   |
| `REGIOES`              | Regiões AWS separadas por vírgula (opcional)  | Todas    |

## Exemplos

### Buscar com valores padrão (todas as regiões)
```bash
./filter_bottlerocket_amis.sh
```

### Versão específica do Bottlerocket
```bash
./filter_bottlerocket_amis.sh 1.58.0
```

### Bottlerocket + versão do Kubernetes específica
```bash
./filter_bottlerocket_amis.sh 1.58.0 1.35
```

### Filtrar apenas regiões específicas
```bash
./filter_bottlerocket_amis.sh 1.58.0 1.35 us-east-1,us-west-2,eu-west-1
```

## Saída

```
Buscando AMIs com o padrao: bottlerocket-aws-k8s-1.35-*-v1.58.0-*
==================================================
Consultando regiao: us-east-1...
  -> 4 AMI(s) encontrada(s)
Consultando regiao: us-west-2...
  -> 4 AMI(s) encontrada(s)

Resultado final:
us-east-1  ami-0abc1234567890  x86_64  bottlerocket-aws-k8s-1.35-x86_64-v1.58.0-c5c37032
us-east-1  ami-0def0987654321  arm64   bottlerocket-aws-k8s-1.35-aarch64-v1.58.0-c5c37032
us-east-1  ami-0aaa1111222233  x86_64  bottlerocket-aws-k8s-1.35-nvidia-x86_64-v1.58.0-c5c37032
us-east-1  ami-0bbb4444555566  x86_64  bottlerocket-aws-k8s-1.35-nvidia-fips-x86_64-v1.58.0-c5c37032
...

Total: 8 AMI(s)
```

## Variantes comuns de AMIs Bottlerocket

| Sufixo no nome          | Descrição                              |
|-------------------------|----------------------------------------|
| `x86_64`                | AMD/Intel 64-bit padrão               |
| `aarch64`               | ARM 64-bit (Graviton)                 |
| `nvidia-x86_64`         | x86_64 com suporte a GPU NVIDIA       |
| `nvidia-fips-x86_64`    | x86_64 com GPU NVIDIA e conformidade FIPS |
