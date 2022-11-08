# SCRIPT BACKUP FTP MIKROTIK

### Crie um script na sua Mikrotik no menu system/script e cole os comandos abaixo:

```
:global arquivo ( [/system identity get name] );

# Gerando Arquivo Backup
:log info "Iniciando Backup..."
/system backup save name=$arquivo
:log info "Backup Concluido"

# Gerando Arquivo export
/export file=$arquivo
:log info "Tempo aproximado 5s"
:delay 5s

# Enviando arquivo de Backup
:log info "Enviando Backup por FTP..."
:log info "Tempo aproximado 5s"
/tool fetch address=ip.do.seu.servidor src-path="$arquivo.rsc" user=seu-usuario mode=ftp password=sua-senha dst-path="$arquivo.rsc" upload=yes
:delay 15s

/tool fetch address=ip.do.seu.servidor src-path="$arquivo.backup" user=seu-usuario mode=ftp password=sua-senha dst-path="$arquivo.backup" upload=yes
:log info "Backup Enviando com Sucesso"

# Deletando Arquivos de Backup
:log info "Deletando Arquivos na RB"
/file remove "$arquivo.backup"
/file remove "$arquivo.rsc"
:log info "Arquivos Deletados"
```
### Agora crie uma regra para que o script execulte automaticamente. Para criar basta ir no menu system/scheduler ou copie
a regra abaixo e cole no new terminal.
```
system scheduler add name=backup-ftp interval=7d on-event=backup-ftp
```
