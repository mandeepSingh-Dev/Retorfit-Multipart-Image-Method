# Retorfit-Multipart-Image-Method


1. Fields for retrofit Method:-> 
val file = File(getFilePath(context, imageUri).toString())
val reqFile = file.asRequestBody("multipart/form-data".toMediaTypeOrNull())
val captionBody = caption.toRequestBody("text/plain".toMediaTypeOrNull())
val profileImageBody = MultipartBody.Part.createFormData("image", file.name, reqFile)

2. Calling ApiService method

Retrofit.ApiService.uploadImage( bearerToken(it.token) ,profileImageBody,   captionBody )

3. ApiService Method :->

@POST("upload/image")
@Multipart
suspend fun uploadImage(
    @Header("Authorization") token: String,
    @Part image: MultipartBody.Part,
    @Part("caption") body: RequestBody): Response<UploadImageResponse>

4. Some extra ApiServices methods

@POST("property-details")
suspend fun getPropertyDetail(
    @Header("Authorization") token: String,
    @Body propertyDetailsRequest: PropertyDetailsRequest
):Response<PropertyDetailsResponse>
