---
title: bootstrap-validator
date: 2016-09-09 17:09:54
tags: Bootstrap
---

>reset重置表单

    $('#btn-reset').click(function() {
        $('#changeForm').data('bootstrapValidator').resetForm(true);
    });
