**TUTORIAL TESTES BENCHMARK**   

**01 Verificando o desempenho da CPU**

```
sysbench --test=cpu run
```

**02 Verificando o teste de Memória Ram**

```
sysbench --test=memory run
```

**03 Verificando I/O DISCO**

```
sysbench --test=fileio --file-test-mode=seqwr run

```

***

***Parte (2) com carga de trabalho.***

**Utilizando carga de trabalho com arquivo "iofile", para teste de I/O de E/S**

**1° Criando os arquivos de teste**
```
sysbench --test=fileio --file-total-size=28G prepare
```
***OBS. Ao criar os arquivos fileio, será criado um conjunto de arquivos para trabalhar, é recomendado que o tamanho seja maior que a memória Ram disponível.***

***2° Executando o comando para testar o I/O***

```
sysbench --test=fileio --file-total-size=28G --file-test-mode=rndrw --max-time=300 --max-requests=0 run
```

***3° Limpando os arquivos fileio criado***

```
sysbench --test=fileio --file-total-size=28G cleanup

```

***4° Usando carga de trabalho da CPU***

```
sysbench --test=cpu --cpu-max-prime=20000 --num-threads=2 run
```

***5° Usando carga de trabalho de memória***

```
sysbench --test=memory --num-threads=4 run
```

**Resultados A:**

**Resultado 01**
<img src="https://user-images.githubusercontent.com/51387190/121817916-955daf80-cc5a-11eb-8aa7-363f6e5571a4.png" alt="Teste básico cpu" title="01" />


**Resultado 02**
<img src="https://user-images.githubusercontent.com/51387190/121818187-11a4c280-cc5c-11eb-8527-5f11d02e32c3.png" alt="Teste básico memória" title="02" />

**Resultado 03**
<img src="https://user-images.githubusercontent.com/51387190/121818545-3863f880-cc5e-11eb-9863-37caaa962ef7.png" alt="IO" title="03" />

** **

**Resultados B:**

**Resultados 1°:**
<img src="https://user-images.githubusercontent.com/51387190/121818694-14ed7d80-cc5f-11eb-9cf1-f5622558cc80.png" alt="IO" title="1°" />

**Resultados 2°:**
<img src="https://user-images.githubusercontent.com/51387190/121818942-ce991e00-cc60-11eb-869a-96a660286a82.png" alt="IO" title="2°" />

**Resultados 3°:**
<img src="https://user-images.githubusercontent.com/51387190/121818964-f2f4fa80-cc60-11eb-82b6-ffcf42d0c339.png" alt="IO" title="3°" />

**Resultados 4°:**
<img src="https://user-images.githubusercontent.com/51387190/121819000-1a4bc780-cc61-11eb-964e-d80884821360.png" alt="IO" title="4°" />

**Resultados 5°:**
<img src="https://user-images.githubusercontent.com/51387190/121819018-3f403a80-cc61-11eb-87da-32c8f64002d9.png" alt="IO" title="5°" />


**Referências:**
https://linuxconfig.org/how-to-benchmark-your-linux-system
https://wiki.gentoo.org/wiki/Sysbench
https://www.howtoforge.com/how-to-benchmark-your-system-cpu-file-io-mysql-with-sysbench

OBS: 
