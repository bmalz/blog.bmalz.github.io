---
layout: post
title: "Rozszerzenie istniejącej funkcji w javascript"
description: ""
category: javascript
tags: [javascript, hack]
comments: true
---
``` javascript
backfill_event = (function() {
	var cached_function = backfill_event;
	return function(form, field_name, value, persid, rel_attr_val, caller_type) {
		cached_function.apply(this, arguments);
		alert('udało się? ' + persid);
	};
}());
```

gdzie `backfill_event` jest istniejącą funkcją javascript przyjmującą argumenty *form, field_name, value, persid, rel_attr_val, caller_type*.
Wykonanie tego kodu spowoduje wywołanie okna wiadomości (alert) po wykonaniu funkcji `backfill_event`. Oczywiście alert może zostać zamieniony na dowolny kod i może korzystać z argumentów funkcji `backfill_event`.