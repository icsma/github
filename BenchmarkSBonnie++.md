
**TUTORIAL TESTES BENCHMARK BONNIE++**   

**BONNIE++**
**01 Verificando o desempenho da CPU**

```
bonnie++
```

```
bonnie++ -d /tmp/ -s 8G -n 0 -m TEST -f -b -u kvmismael
```

**OBS: para gera os resultados em html é necessario copiar os resultados que foram gerados por meio dos comandos anteriores conforme visto no resultado 2 e 4 ou pode ser gerado conforme o comando abaixo!**

```
bonnie++ -d /tmp -s 500 -r 250 -n 2 -m CIALINUX -x 3 -u kvmismael | bon_csv2html > /tmp/RESULTADO.html
```

**Resultados A:**

**Resultado 01**
<img src="https://user-images.githubusercontent.com/51387190/123182067-05431580-d465-11eb-8bc5-b4fbafee3bb3.png" alt="Teste básico cpu" title="01" />

**Resultado 02**
<img src="https://user-images.githubusercontent.com/51387190/123180214-36b9e200-d461-11eb-85d1-70e43eb0d603.png" alt="Teste básico cpu" title="02" />

** **

**Resultados B:**

**Resultado 01**
<img src="https://user-images.githubusercontent.com/51387190/123179706-53094f00-d460-11eb-82e4-3edd6415c821.png" alt="Teste básico cpu" title="02" />
** **
**Resultado 02**
** **
<img src="https://user-images.githubusercontent.com/51387190/123180951-b8f6d600-d462-11eb-9958-7dbc211a4887.png" alt="Teste básico cpu" title="02" />
** **
**Resultado 03**
** **
<img src="https://user-images.githubusercontent.com/51387190/123181735-3bcc6080-d464-11eb-8b5a-70436b8e300b.png" alt="Teste básico cpu" title="02" />
** **
**Resultado 04**
** **
<img src="https://user-images.githubusercontent.com/51387190/123181960-cdd46900-d464-11eb-9cef-71563664284d.png" alt="Teste básico cpu" title="02" />
** **
**Referências:**
https://blog.smallbee.com.br/bonnie-benchmark-de-disco-em-sistemas-linux/
https://blog.smallbee.com.br/bonnie-benchmark-de-disco-em-sistemas-linux/
** **


