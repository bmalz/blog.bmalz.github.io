---
layout: post
title: "Migracja zdalnego repozytorium GIT"
description: ""
category: GIT
tags: [git, migracja]
---
Z różnych niezależnych ode mnie powodów zmuszony zostałem do migracji zdalnego repozytorium GIT z visualstudio.com na lokalną instalacje bitbucket. Sprawa niby prosta ale wiąże się z wykonaniem kilku niezbędnych kroków.

1. Utworzenie nowego repozytorium w docelowym środowisku.
2. Dodanie nowej lokalizacji repozytorium zdalnego:
```shell
git remote add temporary_repo new_remote_repo_url
```
Aby sprawdzić czy nowa lokalizacja została poprawnie dodana można użyć polecenia:
```shell
git remote -v
```
3. Przesłanie lokalnego repozytorium na nowe repozytorium zdalne:
```shell
git push temporary_repo master
```
W tej chwili całe repozytorium lokalne powinno zostać przesłane na nowe repozytorium zdalne.
4. Ponieważ mamy repozytorium na docelowym środowisku można usunąć lokalizację pierwotnego repozytorium zdalnego:
```shell
git remote rm origin
```
5. Dla porządku można zmienić nazwę lokalizacji repozytorium zdalnego na origin:
```shell
git remote rename temporary_repo origin
```