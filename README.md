# Protobuf for SSO

The main code will be written in **Go** and all builds `*.proto` files will be in **Go** format

This project is a set of proto gRPC contracts for use in SSO authorization type projects. It
includes all the main entities and their description from the query side, which developers may need at
the first stages of project development.

It was the gRPC protocol that was chosen from the following main points:
> 1. **Performance:** gRPC provides high performance through the use of Protocol Buffers
     for data serialization and the HTTP/2 binary protocol for message transmission. This allows you to reduce the
     amount
     of data transferred and speed up network interaction.
> 2. **Strong typing:** Protobuffers provide strong data typing, which allows you to define a strict
     message structure and their types. This reduces the likelihood of errors in the development of client and server
     applications.
> 3. **Bidirectional data flow:** gRPC supports data streaming from both client to server and server to
     client. This is especially useful for implementing streaming services such as real-time monitoring or chats.

The project consists of the following hierarchy:
All `*.proto` files that should be expanded and upgraded are located in `./proto/*` and
the generated files are divided into logical blocks. The `*.go` formats are already in the `./gen/go/*` folder and they
are
also further divided into logical blocks.

The command to run the build of Go files from `*.proto` is presented below, in it, you will simply need to substitute
the
path to the file in place of the variables:

```Bash
     protoc -I proto proto/user/user.proto \
     --go_out=./gen/go \
     --go_opt=paths=source_relative \
     --go-grpc_out=./gen/go/ \
     --go-grpc_opt=paths=source_relative
```

To simplify code generation on Go, files were written (Taskfile, Makefile) and they lie at the root of the project, to run them you need to
enter:

Command for `Taskfile.yml`
```Bash
task generate_all
```
Command for `Makefile`
```Bash
make generate_all
```

The `*.proto` files contains a few different and necessary contracts:

1. `./proto/app/app.proto` - file for CRUD app
```Go
service App{
  rpc RegisterApp(RegisterAppRequest) returns(google.protobuf.Empty);
  rpc RemoveApp(RemoveAppRequest) returns(RemoveAppResponse);
  rpc UpdateApp(UpdateAppRequest) returns(UpdateAppResponse);
  rpc AppList(AppListRequest) returns(AppListResponse);
}
```

2. `./proto/user/user.proto` - file for CRUD app
```Go
service User{
  rpc AddNewUser(AddNewUserRequest) returns(google.protobuf.Empty);
  rpc RemoveUser(RemoveUserRequest) returns(RemoveUserResponse);
  rpc UpdateUser(UpdateUserRequest) returns(UpdateUserResponse);
  rpc UserList(UserListRequest) returns(UserListResponse);
}
```

3. `./proto/permission/permission.proto` - file for CRUD permission related with role
```Go
service Permission{
  rpc AddNewPermission(AddNewPermissionRequest) returns(google.protobuf.Empty);
  rpc RemovePermission(RemovePermissionRequest) returns(RemovePermissionResponse);
  rpc UpdatePermission(UpdatePermissionRequest) returns(UpdatePermissionResponse);
  rpc PermissionList(PermissionListRequest) returns(PermissionListResponse);
}
```

4. `./proto/role/role.proto` - file for CRUD role related with user
```Go
service Role{
  rpc AddNewRole(AddNewRoleRequest) returns(google.protobuf.Empty);
  rpc RemoveRole(RemoveRoleRequest) returns(RemoveRoleResponse);
  rpc UpdateRole(UpdateRoleRequest) returns(UpdateRoleResponse);
  rpc RoleList(RoleListRequest) returns(RoleListResponse);
}
```

5. `./proto/sso/sso.proto` - file for auth and register new users
```Go
service Auth{
  rpc Register(RegisterRequest) returns(RegisterResponse) {};
  rpc Login(LoginRequest) returns(LoginResponse) {};
}

```
