troubleshooting_guide

#❗ Error 401: Unauthorized

Causas posibles:
- El token de autenticación no se ha incluido, está mal escrito o ha expirado.

Recomendaciones:
- Verificar que el token personal (PAT) sea válido y esté correctamente copiado.
- Asegurarse de enviar el encabezado de autenticación correcto:
	makefile
	Copiar
	Editar
	Authorization: token <tu_token>
- Confirmar que el token tenga los permisos (scopes) necesarios para acceder a los recursos deseados.

#❗ Error 403: Forbidden (Límite de uso excedido)

Causas posibles:
- Se superó el número permitido de solicitudes por hora:
	60 por hora para usuarios no autenticados.
	5,000 por hora para usuarios autenticados.
Recomendaciones:
- Consultar los encabezados de respuesta para revisar el estado del límite:
X-RateLimit-Remaining: solicitudes restantes.
X-RateLimit-Reset: timestamp en que el límite se reinicia.
	En scripts, utilizar pausas programadas (time.sleep()) si se acerca al límite de uso.

#❗ Error 404: Not Found

Causas posibles:
- El endpoint está mal escrito o el recurso solicitado no existe, es privado o no se tiene acceso.
Recomendaciones:
- Verificar la URL usada.
- Confirmar que el repositorio o recurso esté disponible públicamente o que se cuente con los permisos necesarios.

# 🌀 Consideraciones sobre la paginación
- La API de GitHub limita la cantidad de resultados devueltos por página (por defecto 30, máximo 100).
Buenas prácticas:
- Añadir el parámetro per_page=100 para obtener la mayor cantidad posible de elementos por solicitud.
- Implementar un mecanismo de paginación, por ejemplo:
	python
	Copiar
	Editar
	import requests
	headers = {'Authorization': 'token <tu_token>'}
	base_url = "https://api.github.com/repos/<usuario>/<repositorio>/commits"
	todos_los_commits = []
	for page in range(1, total_pages + 1):
    	url = f"{base_url}?per_page=100&page={page}"
    	response = requests.get(url, headers=headers)
    	if response.status_code == 200:
        todos_los_commits.extend(response.json())
    	else:
        break