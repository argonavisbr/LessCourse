#{9: cores }
Less possui várias funções para geração, combinação, filtragem e mesclagem de cores. Todas geram como resultado cores expressas em CSS no formato hexadecimal `#rrggbb` ou `#rrggbbaa` (exceto `argb()`).

##definição de cores
Várias das funções de definição de cores já existem em CSS, mas várias só existem desde versões mais recentes e não têm suporte em browsers antigos. O Less converte todas em formatos hexadecimais que têm maior suporte.

Função | Recebe | Retorna
--|--|--
`rgb(r,g,b)` |`r` = red, `g` = green, `b` = blue |
`rgba(r,g,b,a)` |`r` = red, `g` = green, `b` = blue, `a` = alpha |
`argb(a,r,g,b)` ||
`hsl(h,s,l)` ||
`hsla(h,s,l,a)` ||
`hsv(h,s,v)` ||
`hsva(h,s,v,a)` ||

##extração de componentes de cores

### componentes rgba
Função | Recebe | Retorna
--|--|--
`red` | |
`green` | |
`blue` | |
`alpha` | |

### componentes hsl
Função | Recebe | Retorna
--|--|--
`hue` | |
`saturation` | |
`lightness` | |

### componentes hsv
Função | Recebe | Retorna
--|--|--
`hsvhue` | |
`hsvsaturation` | |
`hsvvalue` | |

###luma e contrast
Função | Recebe | Retorna
--|--|--
`luma` | |
`contrast` | |

##operações sobre cores individuais
### saturação
Função | Recebe | Retorna
--|--|--
`saturate` | |
`desaturate` | |
`greyscale` | |

### luminância
Função | Recebe | Retorna
--|--|--
`lighten` | |
`darken` | |

### matiz
Função | Recebe | Retorna
--|--|--
`spin` | |

### transparência
Função | Recebe | Retorna
--|--|--
`fadein` | |
`fadeout` | |
`fade` | |


##operações de combinação de cores

Função | Recebe | Retorna
--|--|--
`mix` | |
`multiply` | |
`screen` | |
`overlay` | |
`softlight` | |
`hardlight` | |
`difference` | |
`exclusion` | |
`average` | |
`negation` | |

##exercícios
1. x
2. s
3. d
4. e
5. d
6. 