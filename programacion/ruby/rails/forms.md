http://api.rubyonrails.org/classes/ActionView/Helpers/FormHelper.html


Formulario generado por el scaffold:

La variable f es la que contiene los valores para rellenar los ya existentes:
f.object

Para enviar un formulario a un path determinado: form_tag(search_zombies_path)
http://api.rubyonrails.org/classes/ActionView/Helpers/FormTagHelper.html#method-i-form_tag
<%= form_tag('/posts') do -%>
  <div><%= submit_tag 'Save' %></div>
<% end -%>

<%= form_for(@zombie) do |f| %>
  <% if @zombie.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(@zombie.errors.count, "error") %> prohibited this zombie from being saved:</h2>

      <ul>
      <% @zombie.errors.full_messages.each do |msg| %>
        <li><%= msg %></li>
      <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field">
    <%= f.label :name %><br> <- <label for="zombie_name">Name</label>. Si tiene un error, lo mete en div con class field_with_errors
    <%= f.text_field :name %> <- Depende de si @zombie existe (entre parentesis) o no generará <input name="zombie[name]" size="30" type="text" ( value="Eric" ) />
				 Si tiene algun error pondrá el input dentro de un div con clase field_with_errors para marcarlo de alguna manera (en rojo)
  </div>
  <div class="field">
    <%= f.label :age %><br>
    <%= f.number_field :age %>
  </div>
  <div class="actions">
    <%= f.submit %>
  </div>
<% end %>


Tipos de datos para formularios:

Si no tenemos "do |f|" podemos hacerlo como: 
<%= hidden_field :post, :_checks, value: 1 %> que generará -> <input id="post__checks" name="post[_checks]" type="hidden" value="1">

El tener o no tener |f| implica tener que definir un nuevo parámetro, que definirá el parámetro:
  hidden_field :post, :_checks -> name="post[_checks]"
  f.hidden_field :_checks -> name="post[_checks]"  (si |f| ha nacido de un elemento 'post')

<%= f.text_field :campo %> <- campo de texto
<%= f.text_field :campo, :value => "test" %> <- campo de texto con valor por defecto
:placeholder => "placeholder text"


<%= f.number_field :age %>
  no se puede poner size a un input type="number", se hace con css:
    input[type="number"] {
      width:40px;
    }
<%= f.number_field :max_checks_attempts, min: 1 %>
  el valor mínimo es 1 (podemos poner lo que queramos, pero las flechitas para elegir número es lo que nos pondrá)

<%= f.text_area :campo %> <- campo de texto multilinea
<%= f.text_area :params, cols: 60, rows: 1 %>

<%= f.hidden_field :_destroy %> <- oculto

<%= f.check_box :casado %> <- para booleans

<%= f.radio_button :tipo, 'primero', checked: true %>
<%= f.radio_button :tipo, 'segundo' %>


<%= text_field "post[vip][#{vip.id}]", :name %>
<input id="post_vip_2_name" name="post[vip][2][name]" type="text">


# Para selects, mirar select.md
<%= f.select :gustos, ['musica', 'deporte'] %> <- desplegable
<%= f.select :gustos, [['musica',1], ['deporte',2]] %> <- se muestra el texto, pero se postea el valor numerico
<%= f.select :is_volatile, [['Template',''],['True',1],['False',0]] %>

Select múltiple para un elemento que tiene una relación M-N con projects
<%= f.select :project_ids, @projects.map {|p| [p.name, p.id]}, {}, {:multiple=>true} %>
Es necesario en el controlador modificar el params.require y añadir al final: project_ids: []

Crear select para elegir un elemento al que pertenecemos (belongs_to)
En este ejemplo, el modelo HostGroup tiene declarado, belongs_to :service)
<%= collection_select :host_group, :service_id, @services, :id, :name, :prompt => 'Please select service'  %>
  El primer y segundo parámetro se usan para definir el id y name del <select> (el primer elemento lo podríamos poner a nil)
  En este ejemplo quedaría: <select id="host_group_service_id" name="host_group[service_id]">
  El parametro name es importante para ver a donde se postean los datos (postear a la clase host_group, parametro service_id
  collection_select object, method, collection, value_method, text_method, options = {}, html_options = {}
  http://api.rubyonrails.org/classes/ActionView/Helpers/FormOptionsHelper.html#method-i-collection_select
  Para definir el valor por defecto: ,{:selected => "whatever_value"})
    Para que no falle el valor por defecto: 
      <%= f.collection_select(:project_id, Project.all, :id, :name, @contact.project_id? ? {selected: @contact.project.id} : {}) %>

Si está ya definido, elegimos el definido, si no existe un comando "check_nrpe" elegimos el primero. Si existe, elegimos check_nrpe
<%= f.collection_select :command_id, @commands.order('name ASC'), :id, :name, f.object.command_id || (@commands.find_by name: "check_nrpe").nil? ? {} : {selected: (@commands.find_by name: "check_nrpe").id}, {class: "command_select"} %>


<%= f.password_field :pass %>
<%= f.range_field :cantidad %> <- un slider para seleccionar un valor
<%= f.email_field :email %> <- en los telefonos nos abrirá el teclado listo para escribir un email
<%= f.url_field :website %> <- en los telefonos nos abrirá el teclado listo para escribir una url
<%= f.telephone_field :tlf %> <- en los telefonos nos abrirá el teclado listo para escribir un telefono


## Fields sin submit ##
fields_for @elemento do |e|

<%= f.fields_for :host_groups, @project.host_groups.where(environment_id: @env) do |builder| %>

## Cascada de selects ##
Eligo un valor en un select, y dependiendo de ese, aparecen unas u otras opciones en el siguiente select.
http://railsguides.net/2013/09/05/cascading-selects-with-ajax-in-rails/ <- con este no lo he conseguido, pero puede que me faltase reiniciar el server
  Este hace uso de peticiones json al server

http://pullmonkey.com/2012/08/11/dynamic-select-boxes-ruby-on-rails-3/
  Este inserta código JS en el _form.html.erb, y mediante ajax obtiene lo que tiene que cambiar

Creo app/assets/javascripts/jquery-dynamic-selectable.coffee y meto el código.
Edito app/assets/javascripts/application.coffee y meto las dos líneas.

En las rutas yo prefiero usar las clases que ya existen: 
Pongo:
  resources :projects do
    get 'host_groups'
  end
Para tener la ruta:
project_host_groups GET    /projects/:project_id/host_groups(.:format)       projects#host_groups

Ahora configuro ese controlador para contestar json con los datos necesarios:



## Generar formularios donde se pueden definir un número variable de elementos ##
http://railscasts.com/episodes/196-nested-model-form-part-1?view=asciicast
http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html

class Project < ActiveRecord::Base
  has_many :host_groups, :dependent => :destroy
  accepts_nested_attributes_for :host_groups
  accepts_nested_attributes_for :host_groups, reject_if: proc { |attr| attr['name'].blank? } <- si uno está en blanco no lo guardamos
  ...
end

Ahora editamos el _form de projects
Tras los campos propios del project metemos
  <%= f.fields_for :host_groups do |builder| %>
  <p>
    <%= builder.label :name, "Hostgroup name" %><br />
    <%= builder.text_field :name %>
  </p>
  <% end %>
El solo pillará el project_id y meterá el valor del nuevo project generado.


En el project controller
  def new
    @project = Project.new
    3.times { @project.host_groups.build }
  end

def project_params
  params.require(:project).permit(:name, host_groups_attributes: [ :id, :name, :_destroy ])
end

Con cuando accedamos a projects/new veremos el nombre para el proyecto, y tres campos nuevos para meter hostgroup name.


Para ver los hostgroups asociados al proyecto en el show, metemos:
<ol>
  <% for host_group in @project.host_groups %>
    <li><%= h host_group.name %></li>
  <% end %>
</ol>


Ahora editamos el form para permitir borrar hostgroups de un project.
Añadimos al form builder:
    <%= builder.check_box :_destroy %>
    <%= builder.label :_destroy, "Remove hostgroup" %>

Y el modeo debe aceptar el destroy
accepts_nested_attributes_for :host_groups, reject_if: proc { |attr| attr['name'].blank? }, allow_destroy: true


Cambiamos el código del formulario para usar partials:
project/_form.html.erb, queda:
<%= f.fields_for :host_groups do |builder| %>
  <%= render 'host_group_field', f: builder %>
<% end %>

views/projects/_host_group_field.html.erb
<p>
  <%= f.label :name, "Hostgroup name" %>
  <%= f.text_field :name %>
  <%= f.check_box :_destroy %>
  <%= f.label :_destroy, "Remove hostgroup" %>
</p>


Ahora queremos agregar un 'nested' más por debajo de hostgroups, que será services.
La configuración del model será igual que para la relación project-host_group.
Para meter el formulario, meteremos un partial en el partial de hostgroup:
views/projects/_host_group_field.html.erb
<div class="hostgroup_field">
  <p>
  <%= f.label :name, "Hostgroup name" %>
  <%= f.text_field :name %>  
  <%= f.check_box :_destroy %>
  <%= f.label :_destroy, "Remove hostgroup" %>
  </p>
  <%= f.fields_for :services do |builder| %>
    <%= render 'services_fields', :f => builder %>
  <% end %>
</div>
 
_services_fields.html.erb
<p class="service_field">
  <%= f.label :name, "Service name" %>
  <%= f.text_field :name %>
  <%= f.label :params, "Params" %>
  <%= f.text_field :params %>
  <%= f.check_box :_destroy %>
  <%= f.label :_destroy, "Remove hostgroup" %>
</p>

Ahora modificamos el controlador para que aparezcan, además de tres cajas para hostgroups, 2 services por cada uno.
3.times do 
  host_group = @project.host_groups.build
  2.times { host_group.services.build } 
end

def project_params
  params.require(:project).permit(:name, host_groups_attributes: [ :id, :name, :_destroy, services_attributes: [ :id, :name, :params, :_destroy ]])
end
Esto último es para aceptar los nuevos parámetros que se nos pasan.
Podemos ver que el form hace post de datos tipo:
project[host_groups_attributes][0][services_attributes][0][params]:-w 10 -c 20
Así podemos ver que hace falta tener un params.require con host_groups_attributes, services_attributes y params


Ahora cambiaremos los checkboxes para eliminar elementos por links con javascript:
Donde estaban los checkboxes cambiamos por:
  <%= f.hidden_field :_destroy %>
  <%= link_to "delete", {}, {id: "delete_item"} %>
Lo que hacemos es seguir manteniendo el input de destroy pero oculto, y al pinchar el link, una función marcará ese destroy a 1, y esconderá el elemento.
El script lo metemos en, por ejemplo, app/assets/javascripts/projects.js.coffee
$(document).on 'click', '#delete_service', (event) ->
  event.preventDefault()
  $(this).prev("input[type=hidden]").val("1")
  $(this).parents(".service_field").hide()
$(document).on 'click', '#delete_hostgroup', (event) ->
  event.preventDefault()
  $(this).prev("input[type=hidden]").val("1")
  $(this).parents(".hostgroup_field").hide()




# Generar fields de un objeto sin mostrar los elementos que ya existan
http://stackoverflow.com/questions/14884704/how-to-get-rails-build-and-fields-for-to-create-only-a-new-record-and-not-includ

<%= f.fields_for :services,@project.services.build do |builder| %>
  <%= render 'service_fields', :f => builder %>
<% end %>


Para si mostrar los objetos sería
<%= f.fields_for :services do |builder| %>
  <%= render 'service_fields', :f => builder %>
<% end %>



# Nested forms. Agregar nuevos campos pulsando un botón
http://stackoverflow.com/questions/17839147/dynamically-add-fields-in-rails-with-out-nested-attributes

La idea es generar una nueva fila y dejarla oculta en el código html.
Cada vez que se pinche en un botón, copiará ese código html y los mostrará.

VISTA:
<div id="pruebas">
</div>
<%= link_to "add bulk", {}, {id: "nuevaPrueba"} %>
<div class="hide" style="visibility:hidden" id="nueva_prueba_form">
  <%= f.fields_for :services,@project.services.build,:child_index => "0000" do |builder| %>
    <%= render 'service_fields', :f => builder %>
  <% end %>
</div>

VISTA, partial (simplifcado):
<p class="service_field">
  <%= f.label :name, "Service name" %>
  <%= f.text_field :name %>
</p>

JAVASCRIPT:
$(document).on 'click', '#nuevaPrueba', (event) ->
  event.preventDefault()
  time = event.timeStamp
  regexp = RegExp("0000", "g")
  $("#pruebas").append($("#nueva_prueba_form").html().replace(regexp, time))

