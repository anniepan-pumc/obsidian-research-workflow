# <%= it.title %>

[Zotero](<%= it.backlink %>)  FileLink: <%= it.fileLink %>
<%= it.authors %>
<%= it.tags %>

<%= it.abstractNote.first() %>

#### Annotations：
<%~ include("annots", it.annotations) %>