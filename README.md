# usuarios-linux
script de criação de estrutura de usuários linux


#!/bin/bash

# Verifica se o script está sendo executado como root
if [ "$(id -u)" -ne 0 ]; then
    echo "Este script deve ser executado como root!" >&2
    exit 1
fi

# Passo 1: Criando os diretórios necessários
echo 'Criando diretórios...'

# Verifica se o diretório /publico já existe, caso contrário, cria
if [ ! -d /publico ]; then
    mkdir /publico
    echo "Diretório /publico criado."
else
    echo "Diretório /publico já existe."
fi

# Verifica se o diretório /adm já existe, caso contrário, cria
if [ ! -d /adm ]; then
    mkdir /adm
    echo "Diretório /adm criado."
else
    echo "Diretório /adm já existe."
fi

# Passo 2: Criando os grupos de usuários
echo 'Criando grupos de usuários...'

# Verifica se o grupo já existe
if ! getent group GRP_TESTE1 > /dev/null; then
    groupadd GRP_TESTE1
    echo "Grupo GRP_TESTE1 criado."
else
    echo "O grupo GRP_TESTE1 já existe."
fi

if ! getent group GRP_ADM_TESTE > /dev/null; then
    groupadd GRP_ADM_TESTE
    echo "Grupo GRP_ADM_TESTE criado."
else
    echo "O grupo GRP_ADM_TESTE já existe."
fi

# Passo 3: Criando os usuários e adicionando aos grupos
echo 'Criando usuários e adicionando aos grupos...'

# Verifica se o usuário já existe antes de criar
if ! id roberto > /dev/null 2>&1; then
    useradd roberto -m -s /bin/bash -p $(openssl passwd -1 Senha123) -G GRP_ADM_TESTE
    echo "Usuário roberto criado e adicionado ao grupo GRP_ADM_TESTE."
else
    echo "O usuário roberto já existe."
fi

if ! id lucas > /dev/null 2>&1; then
    useradd lucas -m -s /bin/bash -p $(openssl passwd -1 Senha123) -G GRP_ADM_TESTE
    echo "Usuário lucas criado e adicionado ao grupo GRP_ADM_TESTE."
else
    echo "O usuário lucas já existe."
fi

if ! id ana > /dev/null 2>&1; then
    useradd ana -m -s /bin/bash -p $(openssl passwd -1 Senha123) -G GRP_TESTE1
    echo "Usuário ana criado e adicionado ao grupo GRP_TESTE1."
else
    echo "O usuário ana já existe."
fi

if ! id carla > /dev/null 2>&1; then
    useradd carla -m -s /bin/bash -p $(openssl passwd -1 Senha123) -G GRP_TESTE1
    echo "Usuário carla criado e adicionado ao grupo GRP_TESTE1."
else
    echo "O usuário carla já existe."
fi

# Passo 4: Especificando permissões dos diretórios
echo 'Especificando permissões dos diretórios...'

# Verifica se as permissões já estão configuradas corretamente para /adm
current_permissions=$(stat -c "%a" /adm)
if [ "$current_permissions" -ne 770 ]; then
    chown root:GRP_ADM_TESTE /adm
    chmod 770 /adm
    echo "Permissões de /adm ajustadas para 770."
else
    echo "Permissões de /adm já estão configuradas corretamente."
fi

# Verifica se as permissões já estão configuradas corretamente para /publico
current_permissions=$(stat -c "%a" /publico)
if [ "$current_permissions" -ne 1770 ]; then
    chown root:GRP_TESTE1 /publico
    chmod 1770 /publico
    echo "Permissões de /publico ajustadas para 1770."
else
    echo "Permissões de /publico já estão configuradas corretamente."
fi

# Finalizando o script
echo 'Fim...'

