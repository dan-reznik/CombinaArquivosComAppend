Combina varios arquivos com append
================

``` r
library(tidyverse)
```

Escreve 100 arquivos identicos

``` r
df <- tribble(~x,~y,~z,
        "dan",51,1.73,
        "matheus",26,1.76,
        "xxx",40,2.01)

1:100 %>% walk(~write_csv2(df,sprintf("data/teste_%03d.csv",.x)))
```

Le primeiro arquivo para pegar cabecalho

``` r
fnames <- fs::dir_ls("data",regexp = "teste_")

cabecalho <- read_lines(fnames[1])[1]
file_comb <- "teste_combinado.csv"
write_lines(cabecalho, file_comb)
```

Combina com append

``` r
append_body <- function(file_in, file_out) file_in %>%
  read_lines %>%
  tail(-1) %>% # remove cabeçalho
  write_lines(file_out,append=T)

fnames %>% walk(append_body,file_comb)
```
