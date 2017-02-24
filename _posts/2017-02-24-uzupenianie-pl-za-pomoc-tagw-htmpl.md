---
layout: post
title: "Uzupełnianie pól za pomocą tagów HTMPL"
description: ""
category: SDM
tags: [SDM, HTMPL]
comments: true
---
{% include JB/setup %}
Domyslne wartości pól na formatce można ustawiać na wiele sposobów:
* skryptem javascript
* PRESET, PRESET_REL, ALG_PRESET, ALG_RESET_REL (w adresie wywoływanej strony)
* używając tagów HTMPL, a dokładnie PDM_SET

Zaprezentuje jak wykorzystać ostatni sposób. Sprawdza się on w momentach gdy z jakiś powodów nie możemy lub nie chcemy modyfikować adresu strony, nie potrzebujemy dynamicznie zmieniać wartości podczas już załadowanej strony (javascript) oraz gdy nie potrzebujemy stosować złożonej logiki przy wyborze uzupełnianej wartości.

Do zastosowania tej metody niezbędna będzie sama wartość. Dobrze jeśli mamy do niej bezpośredni uchwyt. Przykładem może być przepisanie wartości z klasyfikacji do treści zgłoszenia (o ile już taka klasyfikacja została wcześniej przypisana). W takim wypadku wystarczy na początku dokumentu umieścić:

```
<PDM_IF "$prop.form_name_3" == "edit" && "$args.category.id" != "">
    <PDM_SET $args.description="$args.category.ss_sym">
</PDM_IF>
```

Spowoduje to każdorazowe przepisanie pola ss_sym z klasyfikacji do pola description podczas podnoszenia zgłoszenia do edycji.


Jednak co w przypadku gdy posiadamy tylko identyfikator obiektu, który nie jest relacją? Można zastosować prosty trik polegajacy na pobraniu obiektu za pomocą tagu *PDM_LIST*. Przykładowo przekazanie dokumentu wiedzy jest wywoływane tylko z id dokumentu wiedzy jako parametr *KEEP.DOC_ID*. Aby dostać się do pól obiektu KD można napisać:
```
<PDM_LIST PREFIX=list_kd FACTORY=KD WHERE="id=$args.KEEP.DOC_ID" LIMIT=1>
    var hdrTitle = "Przekaż dokument <PDM_FMT WIDTH=50 JUSTIFY=WRAP>$list_kd.TITLE</PDM_FMT>";
</PDM_LIST> 
```

Umieszczenie tego fragmentu w *head* spowoduje zmianę tytułu na bardziej czytelną dla użytkownika (w oryginale prezentowany jest id dokumentu wiedzy).


Przypadkiem wymagającym dodatkowego omówienia jest pole typu Lookup. Ponieważ składa się ono z dwóch wartości: nazwy wyświetlanej oraz id elementu wymagane jest podanie obu wartości. Znowu posłóże się przykładem formatki przekazania dokumentu bazy wiedzy. Jednak tym razem będę chciał uzupełnić pole wyboru użytkownika o nazwie *ANALYST_ID*.
```
    <PDM_LIST PREFIX=list_kd FACTORY=KD WHERE="id=$args.KEEP.DOC_ID" LIMIT=1>
        <PDM_SET args.ASSIGNEE_ID="$list_kd.OWNER_ID">
        <PDM_SET args.ASSIGNEE_ID.COMMON_NAME="$list_kd.OWER_ID.COMMON_NAME">
    </PDM_LIST>
```

Jak widać ustawiam wartość ASSIGNEE_ID jako id użytkownika z pola OWNER_ID z obiektu KD oraz ASSIGNEE_ID.COMMON_NAME jako nazwa wyświetlana z tego samego pola. Co ciekawe działa to zarówno dla pola Lookup jak i LookupReadonly. Przy czym należy pamiętać, że tak ustawione pole typu LookupReadonly wymaga utworzenia pola input aby wartość została zapisana razem z zapisem obiektu.

```
    <input type=hidden name=SET.ASSIGNEE_ID value="$args.ASSIGNEE_ID">
```