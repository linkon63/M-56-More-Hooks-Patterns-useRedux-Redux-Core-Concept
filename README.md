# M-56-More-Hooks-Patterns-useRedux-Redux-Core-Concept
import React, { useReducer, useRef } from 'react';
import { PatientReducer, patientState } from '../reducer/patientReducer';

const PatientManagement = () => {
    const nameRef = useRef();
    const [state, dispatch] = useReducer(PatientReducer, patientState,);
    const handleSubmit = e => {
        e.preventDefault();
        dispatch({
            type: 'ADD_PATIENT',
            id: state.patients.length + 1,
            name: nameRef.current.value
        });
        nameRef.current.value = '';
        // console.log(nameRef.current.value);
    }
    return (
        <div>
            <h1>Patient Management : {state.patients.length}</h1>
            <form onSubmit={handleSubmit}>
                <input ref={nameRef} />
            </form>
            {
                state.patients.map(patient => 
                <li 
                key={patient.id}
                onClick={() => dispatch({type: 'REMOVE_PATIENT', id: patient.id})}
                >
                    {patient.name}
                    </li>)
            }
        </div>
    );
};

export default PatientManagement;


export const patientState = {
    patients : []
};

export const PatientReducer = (state,action) => {
    switch(action.type){
        case 'ADD_PATIENT':
            const newPatient ={
                id: action.id,
                name: action.name
            }
            const allPatient = [...state.patients, newPatient];
            return {patients : allPatient};
        case 'REMOVE_PATIENT': 
        console.log(action);
        const remaining = state.patients.filter(pt => pt.id !== action.id);
        const newState = {patients : remaining}
            return newState;
        default :
            return state;
    }
}
