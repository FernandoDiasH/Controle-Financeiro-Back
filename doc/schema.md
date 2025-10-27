# Banco de dados
```
// Use DBML to define your database structure
// Docs: https://dbml.dbdiagram.io/docs

Table usuario {
  id uuid [pk]
  idusuariodependente uuid
  nome text
  email text [not null, unique]
  senha text [not null]
  telefone text [not null]
  documento text [not null]
  idusuarioadd uuid [ref: - usuario.id]
  idusuariodel uuid 
  datahoraadd timestamp
  datahoradel timestamp
}

Ref: usuario.id - usuario.idusuariodependente
Ref: usuario.id < categoria.idusuario
Ref: usuario.id < banco.idusuario
Ref: usuario.id < lancamento.idusuario
Ref: usuario.id < lancamento_recorrencia.idusuario

Table permissao {
  id uuid [pk]
  idmodulo uuid [ref: - modulo.id]
  codigo varchar(50) [unique, not null]
  nome varchar(255) 
  descricao varchar(255)
}

Table modulo {
  id uuid [pk]
  nome varchar(50)
  descricao varchar(50)
}

Table usuario_permissao {
  idusuario uuid 
  idpermissao uuid
  datahoraadd timestamp [default: `now()`]
}

Ref: usuario.id < usuario_permissao.idusuario
Ref: permissao.id < usuario_permissao.idpermissao

Table categoria {
	id integer PK
	idusuario integer
	descricao string
	limite integer
	datahoraadd timestamp
	datahoradel timestamp
}

Table banco {
	id integer PK
	idusuario integer
	descricao string
	tipocartao tipocartao
	padrao boolean
	limitcredito integer
	diafechamento integer
	diavencimento integer
	datahoraadd data
	datahoradel data
}

enum tipo_cartao {
  "débito"
  "crédito"
  "débito/crédito"
  "Alimentação"
  "Refeição"
  "Alimentação/Refeição"
}

table lancamento {
	id integer PK
	idusuario integer 
	idusuariodel integer 
	idcategoria integer
	idbanco integer 
	descricao string
	valor float
	datalancamento timestamp
	tipolancamento tipolancamento
	datahoraadd timestamp
	datahoradel timestamp
}

Ref: lancamento.idcategoria - categoria.id
Ref: lancamento.idbanco - banco.id

enum tipolancamento {
  "individual"
  "dividido"
}

table lancamento_recorrencia {
	id integer PK
	idusuario integer 
	descricao string
	valor integer
	datavencimento integer
	temporecorrencia integer
}

enum tiporecorrencia {
  "mensal"
  "trimestral"
  "semestral"
  "anual"
  "indeterminado"
  "personalizado"
}

```