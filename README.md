# Docker- Utilização Prática no Cenário de Microsserviços

Este projeto provisiona um cluster Docker Swarm usando Vagrant e VirtualBox, com 1 manager e 2 workers. O deploy dos serviços é feito via `docker-compose.yml` como stack.

## Pré-requisitos

- [Vagrant](https://www.vagrantup.com/downloads)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## Subindo o ambiente

1. Clone este repositório e acesse a pasta do projeto.
2. Execute:

   ```sh
   vagrant up
   ```

   Isso irá provisionar 3 VMs: `manager`, `worker1`, `worker2`, instalar Docker e inicializar o Swarm.

## Deploy da aplicação

1. Acesse o manager:

   ```sh
   vagrant ssh manager
   ```

2. No manager, faça o deploy do stack:

   ```sh
   cd /vagrant
   docker stack deploy -c docker-compose.yml appstack
   ```

3. Verifique os serviços:

   ```sh
   docker stack services appstack
   ```

## Acessando a aplicação

- Acesse `http://<IP do manager>` no navegador. O Nginx reverse proxy estará escutando na porta 80.

## Observações

- O diretório `/vagrant` nas VMs é sincronizado com a pasta do projeto no host.
- Para destruir o ambiente:

  ```sh
  vagrant destroy -f
  ```
