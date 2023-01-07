# Retorfit-Multipart-Image-Method


1. Fields for retrofit Method:-> 
val file = File(getFilePath(context, imageUri).toString())

val reqFile = file.asRequestBody("multipart/form-data".toMediaTypeOrNull())

val captionBody = caption.toRequestBody("text/plain".toMediaTypeOrNull())

val profileImageBody = MultipartBody.Part.createFormData("image", file.name, reqFile)

2. Calling ApiService method

Retrofit.ApiService.uploadImage( bearerToken(it.token) ,profileImageBody,   captionBody )

3. ApiService Method  :->

@POST("upload/image")
@Multipart
suspend fun uploadImage( 
@Header("Authorization") token: String,
@Part image: MultipartBody.Part,
@Part("caption") body: RequestBody): Response<UploadImageResponse>

4. ******

       val fNameBody = firstName.toRequestBody("text/plain".toMediaTypeOrNull())
        val lNameBody = lastName.toRequestBody("text/plain".toMediaTypeOrNull())
        val phoneNoBody = phone.toRequestBody("text/plain".toMediaTypeOrNull())
        val addressBody = address.toRequestBody("text/plain".toMediaTypeOrNull())

        val map = mutableMapOf<String, RequestBody>()
        var profileImageBody: MultipartBody.Part? = null
        map["first_name"] = fNameBody
        map["last_name"] = lNameBody
        map["phone_number"] = phoneNoBody
        map["address"] = addressBody

        if (imageUri?.path != null) {

            Log.d("dfkdndfd",imageUri?.path.toString())

            val file = File(context.getRealPathFromUri(imageUri).toString())

            val reqFile = file.asRequestBody("multipart/form-data".toMediaTypeOrNull())

            profileImageBody = MultipartBody.Part.createFormData("image", file.name, reqFile)

        } 
        
         @PUT("profile")
    @Multipart
    suspend fun updateProfile(
        @Header("Authorization") token: String,
        @Part image: MultipartBody.Part?,
        @PartMap partMap: MutableMap<String, RequestBody>
    ): UpdateProfileResponse
       
       ///// Method to get Real path from Uri //////
         fun Context.getRealPathFromUri(uri: Uri?, ): String?
    {
        val contentResolver = this.contentResolver ?: return null
        val extension = uri?.let { getFileExtension(it, this) }

        // Create file path inside app's data dir
        val filePath = File("${this.externalCacheDir}${File.separator}${System.currentTimeMillis()}.${extension}").toString()

        val file = File(filePath)
        try {
            val inputStream = contentResolver.openInputStream(uri!!) ?: return null
            val outputStream: OutputStream = FileOutputStream(file)
            val buf = ByteArray(1024)
            var len: Int
            while (inputStream.read(buf).also { len = it } > 0) outputStream.write(buf, 0, len)
            outputStream.close()
            inputStream.close()
        } catch (e: IOException) {
            println("exception: ${e.message}")
            return null
        }
        return file.absolutePath
    }
       ///////
       
       //// Method to get File Extensions/////
         fun getFileExtension(uri: Uri, context: Context): String? {
        return if (uri.scheme == ContentResolver.SCHEME_CONTENT) {
            val mime = MimeTypeMap.getSingleton()
            mime.getExtensionFromMimeType(context.contentResolver.getType(uri))
        } else {
            MimeTypeMap.getFileExtensionFromUrl(Uri.fromFile(uri.path?.let { File(it) }).toString())
        }

    }

       //////////
        
        ******

5. Some extra ApiServices methods

@POST("property-details")

    suspend fun getPropertyDetail(
    
    @Header("Authorization") token: String,
    
    @Body propertyDetailsRequest: PropertyDetailsRequest

    ):Response<PropertyDetailsResponse>
    
    
    
