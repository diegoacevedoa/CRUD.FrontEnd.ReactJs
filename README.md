# CRUD.FrontEnd.ReactJs
CRUD Front End en ReactJs.

PASOS PARA DESARROLLARLO

1- Crear proyecto con Vite: npm create vite@latest project-name, seleccionamos reactjs y javascript
2- Nos ubicamos en la carpeta del proyecto: cd project-name
3- Instalamos librerías de nodejs: npm install
4- Ejecutamos proyecto para ver que todo funcione OK: npm run dev
5- Instalamos estilos css: npm install react-bootstrap bootstrap --> o cualquier otros estilos y los iconos: npm i react-bootstrap-icons
6- Eliminamos los archivos index.css, app.css, react.svg y los linkeos a esos archivos
7- Limpiamos componente App y agregamos sentencia: import "bootstrap/dist/css/bootstrap.min.css" para que funcione el boostrap, verificamos funcionamiento OK con npm run dev
8- Instalar Sass para estilos, es mejor que css, es scss: npm install sass
9- Instalamos sweetalert2 para los mensajes: npm i sweetalert2
10-Creamos carpetas: api, components, helpers y hooks en carpeta src.
11-Creamos carpetas: modules y ui en carpeta components.
12-Incluimos componentes en la carpeta ui: button, field, label, loading, modal, pagination y table.
13-Inlcuimos el hook useForm en la carpeta hooks para para capturar datos de entrada y sus validaciones.
14-Incluimos el archivo fetch.js en la carpeta helpers para llamar el api del backend.
15-Creamos la carpeta persona en modules y creamos los componentes en esta con rafc: Persona.jsx, PersonaList.jsx, PersonaForm.jsx y el
   archivo index.js para exportar el componente principal Persona.jsx, e invocamos el componente Persona.jsx desde el componente App.jsx.
16-Creamos el archivo constants.js en la carpeta helpers con las rutas del api del backend:

export const API = {
  PERSONA_GET_ALL: "persona",
  PERSONA_CREATE: "persona",
  PERSONA_UPDATE: "persona/{id}",
  PERSONA_DELETE: "persona/{id}",
};


17- Creamos el archivo helpers.js en la carpeta helpers:

export const mapper = (str, type) => {
  if (type === "date") {
    if (str) {
      return new Date(str).toLocaleString();
    }
  }

  return str;
};


18-Creamos la carpeta persona en api y creamos el archivo persona.js para los métodos de llamado del api del back end:

import { API } from "../../helpers/constants";
import { fetchSinToken } from "../../helpers/fetch";

export const getAllPersonas = async () => {
  try {
    const response = await fetchSinToken(API.PERSONA_GET_ALL, {}, "GET");
    const body = await response.json();

    if (response.status === 200) {
      return {
        apiCode: response.status,
        apiData: body,
        apiError: false,
        apiErrors: "",
        apiMessage: body.msg,
      };
    } else {
      return {
        apiCode: response.status,
        apiData: null,
        apiError: true,
        apiErrors: JSON.parse(
          JSON.stringify(
            body.message.includes("Forbidden")
              ? "Usted no posee permisos para realizar esta acción. "
              : body.message
          )
        ).toString(),
        apiMessage: "",
      };
    }
  } catch (error) {
    console.log(error);

    return {
      apiCode: "401",
      apiData: null,
      apiError: true,
      apiErrors:
        error != null && error.toString() === "TypeError: Failed to fetch"
          ? "El sitio no está disponible. "
          : error.toString(),
      apiMessage: "",
    };
  }
};

export const createPersona = async (item) => {
  try {
    const response = await fetchSinToken(
      API.PERSONA_CREATE,
      {
        noDocumento: item.noDocumento,
        nombres: item.nombres,
        apellidos: item.apellidos,
      },
      "POST"
    );
    const body = await response.json();

    if (response.status === 200) {
      return {
        apiCode: response.status,
        apiData: body,
        apiError: false,
        apiErrors: "",
        apiMessage: body.msg,
      };
    } else {
      return {
        apiCode: response.status,
        apiData: null,
        apiError: true,
        apiErrors: JSON.parse(
          JSON.stringify(
            body.message.includes("Forbidden")
              ? "Usted no posee permisos para realizar esta acción. "
              : body.message
          )
        ).toString(),
        apiMessage: "",
      };
    }
  } catch (error) {
    console.log(error);

    return {
      apiCode: "401",
      apiData: null,
      apiError: true,
      apiErrors:
        error != null && error.toString() === "TypeError: Failed to fetch"
          ? "El sitio no está disponible. "
          : error.toString(),
      apiMessage: "",
    };
  }
};

export const updatePersona = async (item) => {
  try {
    const response = await fetchSinToken(
      API.PERSONA_UPDATE.replace("{id}", +item.idPersona),
      {
        idPersona: item.idPersona,
        noDocumento: item.noDocumento,
        nombres: item.nombres,
        apellidos: item.apellidos,
      },
      "PUT"
    );
    const body = await response.json();

    if (response.status === 200) {
      return {
        apiCode: response.status,
        apiData: body,
        apiError: false,
        apiErrors: "",
        apiMessage: body.msg,
      };
    } else {
      return {
        apiCode: response.status,
        apiData: null,
        apiError: true,
        apiErrors: JSON.parse(
          JSON.stringify(
            body.message.includes("Forbidden")
              ? "Usted no posee permisos para realizar esta acción. "
              : body.message
          )
        ).toString(),
        apiMessage: "",
      };
    }
  } catch (error) {
    console.log(error);

    return {
      apiCode: "401",
      apiData: null,
      apiError: true,
      apiErrors:
        error != null && error.toString() === "TypeError: Failed to fetch"
          ? "El sitio no está disponible. "
          : error.toString(),
      apiMessage: "",
    };
  }
};

export const deletePersona = async (id) => {
  try {
    const response = await fetchSinToken(
      API.PERSONA_DELETE.replace("{id}", +id),
      {},
      "DELETE"
    );
    const body = await response.json();

    if (response.status === 200) {
      return {
        apiCode: response.status,
        apiData: body,
        apiError: false,
        apiErrors: "",
        apiMessage: body.msg,
      };
    } else {
      return {
        apiCode: response.status,
        apiData: null,
        apiError: true,
        apiErrors: JSON.parse(
          JSON.stringify(
            body.message.includes("Forbidden")
              ? "Usted no posee permisos para realizar esta acción. "
              : body.message
          )
        ).toString(),
        apiMessage: "",
      };
    }
  } catch (error) {
    console.log(error);

    return {
      apiCode: "401",
      apiData: null,
      apiError: true,
      apiErrors:
        error != null && error.toString() === "TypeError: Failed to fetch"
          ? "El sitio no está disponible. "
          : error.toString(),
      apiMessage: "",
    };
  }
};


19-Creamos el archivo map.js en la careta api/persona para mapear las columnas de las grids y el archivo index.js para exportar los métodos de la carpeta api/persona:

import { mapper } from "../../helpers/helpers";

export const mapHeadPersona = {
  columns: [
    { label: "Acciones", name: "acciones", format: mapper, type: "text", editable: false  },
    { label: "NoDocumento", name: "noDocumento", format: mapper, type: "text", editable: false },
    { label: "Nombres", name: "nombres", format: mapper, type: "text", editable: false },
    { label: "Apellidos", name: "apellidos", format: mapper, type: "text", editable: false  },
  ],
};

20-Crear variables de entorno en src .env.development y .env.production:  VITE_APP_API_URL=http://localhost:3000/api

21-Modificar componente PersonaList.jsx

import React, { useMemo, useEffect, useCallback } from "react";
import PropTypes from "prop-types";
import { Stack } from "react-bootstrap";
import {
  deletePersona,
  getAllPersonas,
  mapHeadPersona,
} from "../../../api/persona";
import Table from "../../ui/table";
import { PencilSquare, Trash } from "react-bootstrap-icons";
import Swal from "sweetalert2";
import { Loading } from "../../ui/loading";

export const PersonaList = ({
  loading,
  setLoading,
  setTitleModal,
  setIsNewModal,
  setShowModal,
  setActiveForm,
  allData,
  setAllData,
}) => {
  useEffect(() => {
    handleGetAllPersonas();
  }, []);

  const handleEdit = useCallback((item) => {
    setTitleModal("Editar Registro");
    setIsNewModal(false);
    setShowModal(true);
    setActiveForm(item);
  });

  const handleDelete = useCallback((id) => {
    Swal.fire({
      title: "Confirmación!",
      text: "Está seguro de querer eliminar el registro?",
      icon: "warning",
      showConfirmButton: true,
      showCancelButton: true,
    }).then((result) => {
      if (result.isConfirmed) {
        handleDeletePersona(id);
      }
    });
  });

  const handleGetAllPersonas = useCallback(async () => {
    setLoading(true);

    const response = await getAllPersonas();

    if (response.apiError) {
      Swal.fire(
        "Error",
        response.apiMessage + " " + (response.apiErrors ?? ""),
        "error"
      );

      setAllData([]);
    } else {
      if (response.apiData != null && response.apiData.data.length > 0) {
        const list = [];
        response.apiData.data.forEach((item) => {
          list.push({
            idPersona: item.idPersona,
            noDocumento: item.noDocumento,
            nombres: item.nombres,
            apellidos: item.apellidos,
          });
        });

        setAllData(list);
      } else {
        Swal.fire(
          "Advertencia",
          "La consulta no retornó registros. ",
          "warning"
        );

        setAllData([]);
      }
    }

    setLoading(false);
  });

  const handleDeletePersona = useCallback(async (id) => {
    setLoading(true);

    const response = await deletePersona(id);

    if (response.apiError) {
      Swal.fire(
        "Error",
        response.apiMessage + " " + (response.apiErrors ?? ""),
        "error"
      );
    } else {
      Swal.fire("OK!", "El registro se ha eliminado exitosamente.", "success");

      const newList = allData.filter((item) => item.idPersona !== id);

      setAllData(newList);
    }

    setLoading(false);
  });

  const getColums = useMemo(() => {
    return mapHeadPersona.columns.map((column) => {
      return {
        key: column.name,
        header: column.label,
        render: (row, rowIndex) => {
          if (column.name === "acciones") {
            return (
              <Stack direction="horizontal" className="float-start">
                <PencilSquare
                  size={20}
                  onClick={() => handleEdit(row)}
                  title="Editar"
                  tabIndex={1}
                />
                <Trash
                  size={20}
                  onClick={() => handleDelete(row.idPersona)}
                  title="Eliminar"
                  tabIndex={2}
                />
              </Stack>
            );
          }

          if (!column.editable) {
            return row[column.name];
          }
        },
      };
    });
  });

  return (
    <>
      <Table
        count={allData.length}
        data={allData}
        columns={getColums}
        pagination={false}
      />
      <Loading show={loading} />
    </>
  );
};

PersonaList.propTypes = {
  loading: PropTypes.bool,
  setLoading: PropTypes.func,
  setTitleModal: PropTypes.func,
  setIsNewModal: PropTypes.func,
  setShowModal: PropTypes.func,
  setActiveForm: PropTypes.func,
  allData: PropTypes.array,
  setAllData: PropTypes.func,
};





22-Modificar componente PersonaForm.jsx


import { useCallback, useEffect, useMemo, useRef, useState } from "react";
import PropTypes from "prop-types";
import Button from "../../ui/button";
import Modal from "../../ui/modal";
import { useForm } from "../../../hooks/useForm";
import Field from "../../ui/field";
import { Container, Form, Col, Row } from "react-bootstrap";
import { BoxArrowLeft, Save2 } from "react-bootstrap-icons";
import { Loading } from "../../ui/loading";
import { createPersona, updatePersona } from "../../../api/persona";
import Swal from "sweetalert2";

const defaultValidationValues = {
  noDocumento: { invalid: false, message: "" },
  nombres: { invalid: false, message: "" },
  apellidos: { invalid: false, message: "" },
};

const validationMessages = {
  noDocumento: "El No Documento es requerido.",
  nombres: "Los Nombres son requeridos.",
  apellidos: "Los Apellidos son requeridos.",
};

export const PersonaForm = ({
  titleModal,
  isNewModal,
  showModal,
  setShowModal,
  activeForm,
  allData,
  setAllData,
}) => {
  const [loadingModal, setLoadingModal] = useState(false);

  const [
    formValues,
    validationValues,
    valid,
    handleInputChange,
    triggerValidation,
    reset,
  ] = useForm(activeForm, defaultValidationValues, validationMessages, true);

  //Guarda un valor mutable
  const activeId = useRef(activeForm.idPersona);

  //Mostrar los datos en pantalla cada que seleccionan una registro diferente a la inicial
  useEffect(() => {
    //Si id cambia, entonces se renderiza el compononte de y se hace esto para evitar ciclo infinito
    if (activeForm.idPersona !== activeId.current) {
      reset(activeForm);
      activeId.current = activeForm.idPersona;
    }
  }, [activeForm, reset]);

  const handleOnSaveModal = useCallback((e) => {
    e.preventDefault();

    triggerValidation();

    if (
      !valid ||
      formValues.noDocumento === "" ||
      formValues.nombres === "" ||
      formValues.apellidos === ""
    ) {
      return;
    }

    if (isNewModal) {
      handleCreatePersona();
    } else {
      handleUpdatePersona();
    }
  });

  const handleCreatePersona = useCallback(async () => {
    setLoadingModal(true);

    const response = await createPersona(formValues);

    if (response.apiError) {
      Swal.fire(
        "Error",
        response.apiMessage + " " + (response.apiErrors ?? ""),
        "error"
      );
    } else {
      Swal.fire("OK!", "El registro se ha creado exitosamente.", "success");

      const newData = { idPersona: response.apiData.idPersona, ...formValues };

      setAllData([newData, ...allData]);
    }

    setLoadingModal(false);
    setShowModal(false);
  });

  const handleUpdatePersona = useCallback(async () => {
    setLoadingModal(true);

    const response = await updatePersona(formValues);

    if (response.apiError) {
      Swal.fire(
        "Error",
        response.apiMessage + " " + (response.apiErrors ?? ""),
        "error"
      );
    } else {
      Swal.fire(
        "OK!",
        "El registro se ha actualizado exitosamente.",
        "success"
      );

      const newList = allData.map((item) =>
        item.idPersona === formValues.idPersona ? formValues : item
      );

      setAllData(newList);
    }

    setLoadingModal(false);
    setShowModal(false);
  });

  const handleOnCloseModal = useCallback(() => {
    setShowModal(false);
  });

  const getBodyModal = useMemo(() => {
    return (
      <Container className="p-0" fluid>
        <Form
          id="formPersona"
          onSubmit={handleOnSaveModal}
          className="animate__animated animate__fadeIn animate__faster"
          noValidate
        >
          <Row>
            <Col
              lg={{ span: 4, offset: 0 }}
              md={{ span: 4, offset: 0 }}
              sm={{ span: 4, offset: 0 }}
              xs={{ span: 8, offset: 1 }}
            >
              <Field
                id="noDocumento"
                label="No Documento"
                placeholder="Ingrese No Documento"
                onChange={handleInputChange}
                value={formValues.noDocumento}
                autoFocus={true}
                autoComplete="off"
                type="text"
                disabled={loadingModal}
                required
                error={validationValues.noDocumento.message}
                isInvalid={validationValues.noDocumento.invalid}
                index={1}
              />
            </Col>
            <Col
              lg={{ span: 4, offset: 0 }}
              md={{ span: 4, offset: 0 }}
              sm={{ span: 4, offset: 0 }}
              xs={{ span: 8, offset: 1 }}
            >
              <Field
                id="nombres"
                label="Nombres"
                placeholder="Ingrese Nombres"
                onChange={handleInputChange}
                value={formValues.nombres}
                autoFocus={true}
                autoComplete="off"
                type="text"
                disabled={loadingModal}
                required
                error={validationValues.nombres.message}
                isInvalid={validationValues.nombres.invalid}
                index={2}
              />
            </Col>
            <Col
              lg={{ span: 4, offset: 0 }}
              md={{ span: 4, offset: 0 }}
              sm={{ span: 4, offset: 0 }}
              xs={{ span: 8, offset: 1 }}
            >
              <Field
                id="apellidos"
                label="Apellidos"
                placeholder="Ingrese Apellidos"
                onChange={handleInputChange}
                value={formValues.apellidos}
                autoFocus={true}
                autoComplete="off"
                type="text"
                disabled={loadingModal}
                required
                error={validationValues.apellidos.message}
                isInvalid={validationValues.apellidos.invalid}
                index={3}
              />
            </Col>
          </Row>
        </Form>
      </Container>
    );
  });

  const getFooterModal = useMemo(() => {
    return (
      <>
        <Button
          className="mb-3"
          variant="secondary"
          disabled={loadingModal}
          tabIndex={4}
          onClick={handleOnCloseModal}
          icon={<BoxArrowLeft />}
        >
          Cerrar
        </Button>
        <Button
          className="mb-3"
          variant="primary"
          disabled={loadingModal}
          tabIndex={5}
          icon={<Save2 />}
          type="submit"
          form="formPersona"
        >
          Guardar
        </Button>
      </>
    );
  });

  return (
    <>
      <Modal
        body={getBodyModal}
        foot={getFooterModal}
        head={titleModal}
        onClose={handleOnCloseModal}
        show={showModal}
        size="lg"
      />
      <Loading show={loadingModal} />
    </>
  );
};

PersonaForm.propTypes = {
  titleModal: PropTypes.string,
  isNewModal: PropTypes.bool,
  showModal: PropTypes.bool,
  setShowModal: PropTypes.func,
  activeForm: PropTypes.any,
  allData: PropTypes.array,
  setAllData: PropTypes.func,
};






23-Modificar componente Persona.jsx



import React, { useState, useCallback } from "react";
import { Container, Col, Row } from "react-bootstrap";
import Label from "../../ui/label/Label";
import { PersonaList } from "./PersonaList";
import Button from "../../ui/button";
import { PersonaForm } from "./PersonaForm";
import { PlusCircle } from "react-bootstrap-icons";

const defaultFormValues = {
  idPersona: 0,
  noDocumento: "",
  nombres: "",
  apellidos: "",
};

export const Persona = () => {
  const [loading, setLoading] = useState(false);
  const [titleModal, setTitleModal] = useState("");
  const [isNewModal, setIsNewModal] = useState(true);
  const [showModal, setShowModal] = useState(false);
  const [activeForm, setActiveForm] = useState();
  const [allData, setAllData] = useState([]);

  const handleNew = useCallback(() => {
    setTitleModal("Agregar Registro");
    setIsNewModal(true);
    setShowModal(true);
    setActiveForm(defaultFormValues);
  });

  return (
    <>
      <Container className="p-0" fluid>
        <Row>
          <Col
            lg={{ span: 10, offset: 0 }}
            md={{ span: 9, offset: 0 }}
            sm={{ span: 9, offset: 0 }}
            xs={{ span: 5, offset: 0 }}
          >
            <Label
              align="left"
              hasMargin
              type={2}
              value="Persona"
              variant="title"
            />
          </Col>
          <Col
            lg={{ span: 2, offset: 0 }}
            md={{ span: 3, offset: 0 }}
            sm={{ span: 3, offset: 0 }}
            xs={{ span: 6, offset: 0 }}
          >
            <Button
              className="mb-2 float-end"
              variant="primary"
              disabled={loading}
              tabIndex={1}
              icon={<PlusCircle size={20} title="Añadir nuevo" />}
              type="submit"
              onClick={handleNew}
            >
              Nuevo
            </Button>
            {showModal && (
              <PersonaForm
                titleModal={titleModal}
                isNewModal={isNewModal}
                showModal={showModal}
                setShowModal={setShowModal}
                activeForm={activeForm}
                allData={allData}
                setAllData={setAllData}
              />
            )}
          </Col>
        </Row>
        <Row>
          <Col>
            <PersonaList
              loading={loading}
              setLoading={setLoading}
              setTitleModal={setTitleModal}
              setIsNewModal={setIsNewModal}
              setShowModal={setShowModal}
              setActiveForm={setActiveForm}
              allData={allData}
              setAllData={setAllData}
            />
          </Col>
        </Row>
      </Container>
    </>
  );
};